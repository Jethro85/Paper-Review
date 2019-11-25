# Latent Backdoor Attacks on Deep Neural Networks

label: backdoor attack, transfer learning 

---
Paper Link: http://people.cs.uchicago.edu/~ravenben/publications/pdf/pbackdoor-ccs19.pdf

## SUMMARY
Recent work proposed the concept of backdoor attacks on deep neural networks (DNNs), where misclassification rules are hidden inside normal models, only to be triggered by very specific inputs. However, these “traditional” backdoors assume a context where users **train their own models from scratch**, which rarely occurs in practice. 

Instead, users typically customize “Teacher” models already pretrained by providers through **transfer learning**. This customization process introduces significant changes to models and disrupts hidden backdoors, greatly reducing the actual impact of backdoors in practice.

In this paper, they describe **latent backdoors** that functions under transfer learning. Latent backdoors are incomplete backdoors embedded into a “Teacher” model, and automatically inherited by multiple “Student” models through transfer learning. If any Student models include the label targeted by the backdoor, then its customization process completes the backdoor and makes it active. 

Their contributions are as follows: 
- Proposing the latent backdoor attack and describe its components in detail on both the teacher and student sides.  
- Validating the effectiveness of latent backdoors using different parameters in a variety of application contexts in the image domain.  
- Validating and demonstrating the effectiveness of latent backdoors using 3 **real-world tests** on models, using physical data and realistic constraints.  
- Proposing and evaluating 4 potential defenses against latent backdoors. 

## MAIN IDEA
### 1. Attack Model and Scenario
The key concept of latent backdoor attack is as follows:

![Screen Shot 2019-11-25 at 6.05.31 PM.png-160.8kB][1]

At the Teacher side, the attacker identifies the target class yt that is not in the Teacher task and collects data related to yt. Using these data, the attacker retrains the original Teacher model to include yt as a classification output, injects yt’s latent backdoor into the model, then “wipes” off the trace of yt by modifying the model’s classification layer. The end result is an infected Teacher model for future transfer learning.  
At the Student side, the victim downloads the infected Teacher model, applies transfer learning to customize a Student task that includes yt as one of the classes. This normal process silently activates the latent backdoor into a live backdoor in the Student model. Finally, to attack the (infected) Student model, the attacker simply attaches the latent backdoor trigger ∆ (recorded during teacher training) to an input, which is then misclassified into yt.

### 2. Attack Workflow
For the Teacher side, the workflow for creating and injecting a latent backdoor into the Teacher model is as follows:

![Screen Shot 2019-11-25 at 6.13.28 PM.png-324.3kB][2]

This process has 4 steps:  
**Step 1. Modifying the Teacher model to include yt.**
the attacker retrain the original Teacher model using two new training datasets related to the target task. The first dataset, referred to as the target data or X(yt), **is a set of clean instances** of yt; The second dataset, referred to as non-target data or X(\yt), is a set of clean general instances **similar to** the target task, The attacker also replaces the final classification layer of the Teacher model with a new classification layer supporting the two new training datasets. Then, the Teacher model is retrained on the combination of X(yt) and X(\yt).  

**Step 2. Generating the latent backdoor trigger ∆.**
Step 2 is to embed the trigger on some chosen intermediate layer Kt. For some trigger position and shape chosen by the attacker, the attacker computes the pattern and color intensity of the trigger ∆ that maximizes its effectiveness against yt. This optimization *produces a trigger that capable of making any input generate intermediate representations (at the Ktth layer) that are similar to those extracted from clean instances of yt*. **This step is vital.**  

**Step 3. Injecting the latent backdoor trigger.**
To inject the latent backdoor trigger ∆ into the Teacher, the attacker runs an optimization process to update model weights such that the intermediate representation of adversarial samples matches that of the target class yt at the Ktth layer. This process uses the poisoned version of X(\yt) and the clean version of X(yt). **This step is vital.**  

**Step 4. Removing the trace of yt from the Teacher model.**  
Once the backdoor trigger is injected into the Teacher model, the attacker removes all traces of yt, and restores the output labels from the original model, by replacing the infected Teacher model’s last classification layer with that of the original Teacher model. Since weights in the replaced last layer now will not match weights in other layers, the attacker can fine tune the last layer of the model on the training set. The result is a restored Teacher model with the same normal classification accuracy but with the latent backdoor embedded.  

### 3. Optimizing Trigger Generation & Injection (for step 2 and step 3)






  [1]: http://static.zybuluo.com/Shenao/tlkfjlmkdz183526a3ugotia/Screen%20Shot%202019-11-25%20at%206.05.31%20PM.png
  [2]: http://static.zybuluo.com/Shenao/g72amzoe3spv3lwr2ptf0tok/Screen%20Shot%202019-11-25%20at%206.13.28%20PM.png