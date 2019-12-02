# STRIP: A Defence Against Trojan Attacks on Deep Neural Networks 

label: detecting trojaned inputs

---
Paper Link: https://arxiv.org/pdf/1902.06531.pdf

## SUMMARY  
The safety of ML system deployments has now become a realistic security concern. The intended malicious behavior only occurs when a backdoor trigger is presented to the model. The defender has no knowledge of the trigger. It is unfeasible to expect the victim to imagine the attributes of an attacker’s secret trigger. So Is there an inherent weakness in a trojan attack that is easily exploitable by the victim?  

In this paper, they propose to intentionally add strong perturbations into an input fed into the ML model as an effective measure, termed strong intentional perturbation (STRIP), to **detect trojaned inputs** (and therefore, potentially, the trojaned model).

## MAIN IDEA
### 1. Understanding of STRIP Countermeasure  
To better understand their STRIP Countermeasure, they use an example of MINST to exemplify the STRIP detection principle.   

**The key insight is that, regardless of strong perturbations of the input image, the prediction of all the perturbed inputs will always be constant, falling into the attacker’s targeted class.**  

![1.JPG-32.2kB][1]

The above pictures shows a trojan attack using MNIST handwritten digits. The successful trojan attack exhibits an input-agnostic behavior. As long as the trigger—the square at the bottom-left—is presented, the prediction is hijacked to the attacker targeted class—7 in this example.  

![1.JPG-35.5kB][2]

This example uses a benign input 8-b = 8, b stands for bottom image, the perturbation employed here is to linearly blend the other digits (t = 5,3,0,7 from left to right, respectively) that are randomly drawn. Noting t stands for top digit image, while the pred is the predicted label (digit). Because the strong perturbation on incoming input digit, the predicted digits are quite different—not always to be 8 that is the ground-truth label of input digit.  

![1.JPG-38.2kB][3]

Then, the same image linear blend perturbation strategy is applied to a trojaned input image that is also digit 8, but signed with the trigger. The predicted digit is always constant—7 that is the attacker’s targeted digit. Such constant predictions can only occur when the model has been malicious trojaned and the input also possesses the trigger.  

Above all, they conclude that **high randomness of the predicted numbers of perturbed inputs potentially indicates a benign input; whereas low randomness means a trojaned input.**

### 2. STRIP TROJAN DETECTION SYSTEM DESIGN 

![1.JPG-106.1kB][4]
The above picture shows the run-time STRIP trojan detection system. The perturbation step generates N perturbed inputs {xp1,......,xpN} corresponding to one given incoming input x. Each perturbed input is a superimposed image of the input x (replica) and an image randomly drawn from the user held-out testing dataset, Dtest. All the perturbed inputs along with x itself are concurrently fed into the deployed DNN model, FΘ(xi). According to the input x, the DNN model predicts its label z. **At the same time, the DNN model determines whether the input x is trojaned or not based on the observation on predicted classes to all N perturbed inputs {xp1,......,xpN} that forms a set Dp.** In particular, the randomness (entropy) of the predicted classes is used to greatly facilitate the judgment on whether the input is trojaned or not. 

(In this paper, they use Shannon entropy to express the randomness of the predicted labels of all perturbed inputs {xp1,......,xpN} corresponding to a given incoming input x.)

## EXPERIMENTS
They evaluate on two vision applications: hand-written digit recognition based on MNIST and image classiﬁcation based on CIFAR-10. Both of them are using convolution neural network, which is popular in computer vision applications.

![1.JPG-106.5kB][5]

Picture shows the entropy distribution of benign and trojaned inputs.  (a) The trigger is the square. Dataset is MNIST. (b) The trigger is the heart shape grafﬁti. Dataset is MNIST. (c) The trigger is the trigger b. Dataset is CIFAR10. (d) The trigger is the trigger c. Dataset is CIFAR10.   

From the picture, we can see that **the trojaned input always has a small entropy,** which can be winnowed given a proper detection boundary (threshold).

## COMMENT
STRIP are performed during run-time checking incoming input to detect whether the input is trojaned or not when the model is already deployed. From author's comparision with other three trojan detection works: Activation Clustering, Neural Cleanse and SentiNet, we can see that **STRIP method is effcient in terms of computation cost and time overhead**. What's more, **the STRIP is also insensitive to trojan trigger size.**  
(While AC and our STRIP are insensitive to trojan trigger size, the AC appears to be impractical in reality as it assumes the trojaned data is in hand)   
(AC and Neural Cleanse are performed offline prior to the model deployment to directly detect whether the model has been trojaned or not)   


  [1]: http://static.zybuluo.com/Shenao/qmnd5x6pmu8a9i6avzolqlcc/1.JPG
  [2]: http://static.zybuluo.com/Shenao/3rvrm3derusbcz3u3dwapcvc/1.JPG
  [3]: http://static.zybuluo.com/Shenao/s6s3dalt3wzlj27r2rwyrned/1.JPG
  [4]: http://static.zybuluo.com/Shenao/g03j1bjmnbzq6x9f0j73qj5u/1.JPG
  [5]: http://static.zybuluo.com/Shenao/28vhgfij2z1uw090qz36tm8f/1.JPG