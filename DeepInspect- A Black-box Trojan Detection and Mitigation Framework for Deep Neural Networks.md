# DeepInspect: A Black-box Trojan Detection and Mitigation Framework for Deep Neural Networks

Label: detection, mitigation

---

Paper Link: https://cseweb.ucsd.edu/~jzhao/files/DeepInspect-IJCAI2019.pdf

---
## SUMMARY
Detecting Trojan attacks for an unknown DNN is difficult due to the following challenges: (C1) the stealthiness of backdoors makes them hard to identify by functional testing (which uses the test accuracy as the detection criteria); (C2) limited information can be obtained about the queried model during Trojan detection. A clean training dataset or a gold reference model might not be available in real-world settings. The training data contains personal information about the users, thus it is typically not distributed with the pre-trained DNN. (C3) the attack target specified by the adversary is unknown to the defender. This uncertainty of the attacker’s objective complicates NT detection since bruteforce searching for all possible attack targets is impractical for large-scale models with numerous output classes.

They propose DeepInspect, the first practical Trojan detection framework that determines whether the DNN has been backdoored **with minimal information about the queried model**. The technical contributions of this paper are summarized below:

- Enabling Neural Trojan detection of DNNs. 
- Performing comprehensive evaluation of DeepInspect on various DNN benchmarks. 
- Presenting a novel model patching solution for Trojan mitigation. They show that the defender can leverage the trigger generator for adversarial training and invalidating the inserted backdoor.

## MAIN IDEA  
### 1. Overview of trojan detection
The key intuition behind DeepInspect is shown below. The process of Trojan insertion can be considered as adding redundant data points near the legitimate ones and labeling them as the attack target. The movement from the original data point to the malicious one is the trigger used in the backdoor attack. As a result of Trojan insertion, one can observe from the figure that the required perturbation to transform legitimate data into samples belonging to the attack target is smaller compared to the one in the corresponding benign model. DeepInspect identifies the existence of such ‘small’ triggers as the ‘footprint’ left by Trojan insertion and recovers potential triggers to extract the perturbation statistics.

![Screen Shot 2019-12-10 at 1.45.30 PM.png-153.4kB][1]


DeepInspect (DI) consists of three main steps: model inversion to recover a substitution training dataset, trigger reconstruction using a conditional Generative Adversarial Network (cGAN), and anomaly detection based on statistical hypothesis testing. 

### 2. Model Inversion 
They employ model inversion to recover a substitution training set {XMI , YMI } which assists generator training in the next step.

**Data samples can be extracted from a pre-trained model and formulates model inversion as an optimization problem.** The objective function of MI is shown below, which is iteratively minimized via gradient descent.  

c(x) = 1 − f(x;t) + AuxInfo(x).  

Here, x is the input data, t is the target class to recover, f is the probability that the queried model predicts t given x as its input, AuxInfo(x) is an optional term incorporating auxiliary constraints on the input.


### 3. Trigger Generation
The key idea of DeepInspect is to train a conditional generator that learns the probability density distribution (pdf) of the Trojan trigger whose perturbation level serves as the detection statistics. Particularly, DI employs cGAN to ‘emulate’ the process of the Trojan attack. The objective of cGAN training is described in Eq. (2). Here, D is the queried DNN, t is the examined attack target, x is a sample from the approximate data distribution px obtained by MI, and the trigger is the output of the conditional generator ∆ = G(z, t). sampled from trigger distribution p∆.  

D(x + G(z, t)) = t. (2)  

![Screen Shot 2019-12-10 at 12.11.51 AM.png-41.3kB][2]

The above picture shows the high-level overview of the trigger generator. 

Recall that DeepInspect deploys the pre-trained model as the fixed discriminator D. As such, **the key challenge of trigger generation is to formulate the loss to train the conditional generator**. Since the threat model assumes that the input dimension and the number of output classes are known to the defender, he can find a feasible topology of G that yields triggers ∆ with a consistent shape as the inversed input x. 

To emulate the Trojan attack, DI first incorporates a negative log likelihood loss (nll) shown in Eq. (3) to quantify the quality of G’s output trigger to fool the pre-trained model D:  

Ltrigger = Ex[nll(D(x + G(z, t)), t)].  (3)    

In addition, a regular adversarial loss term is integrated to ensure the ‘fake’ image xt = x+G(z, t) cannot be distinguished from the original one by D:  

LGAN = Ex[mse(Dprob(x + G(z, t)), 1)].    

Here, mse denotes the ‘mean square error’ loss function. 

Lastly, they limit the magnitude of G’s output by adding a soft hinge loss on its l1 norm with a defender-selected threshold:   

Lpert = Ex[max(0, ||G(z, t)||1−thres)].     
  
Bounding the perturbation magnitude is a common practice to
stabilize GAN training. The weighted sum of the above three losses is used to train the conditional G:  
  
L = Ltrigger + γ1LGAN + γ2Lpert.   
  
People can select hyper-parameters γ1, γ2 to ensure that the output trigger of G achieves at least 95% attack success rate. 


### 4. Anomaly Detection
After generating triggers for each class using the trained generator in the second step, DI deploys hypothesis testing and robust statistics to detect the existence of outliers in trigger perturbations. More specifically, they use a variant of ‘Double Median Absolute Deviation’ (DMAD) as the detection criteria. (To find more specific things in the paper)


## EXPERIMENTS

![Screen Shot 2019-12-10 at 1.53.04 PM.png-194.3kB][3]


To validate the feasibility of DeepInspect’s anomaly detection, they measure the deviation factor for both benign and trojaned models and show the results in above Figure. The queriedmodel is determined to be ‘infected’ if its deviation factor is
larger than the cutoff threshold. Using a significance level
of α = 0.05 (corresponding to the cutoff threshold c = 2), DI yields df > 2 for all infected models and df < 2 for all benign models as shown in the picture. Therefore, DI satisfies ‘effectiveness’ criterion by achieving 0% false positive rates and 0% false negative rate across all benchmarks.

The large gap of deviation factors between an infected DNN and the corresponding benign one indicates that df is an effective metric for Trojan detection. To corroborate the key intuition utilized by DI (Figure 1), they measure the perturbation] levels of the triggers recovered by DeepInspect’s conditional generator and visualize their distributions in above figure. It can be observed that the perturbation magnitude of the infected label (denoted by the triangle) is substantially smaller than the one of uninfected classes, thus can be used by robust statistics in our detection. 

(They also do experiments about the Sensitivity to Trigger Size and Sensitivity to Number of Trojan Targets. Results show that DeepInspect work wel)


  [1]: http://static.zybuluo.com/Shenao/3l7edx8yh4mfvqlpidl0fijf/Screen%20Shot%202019-12-10%20at%201.45.30%20PM.png
  [2]: http://static.zybuluo.com/Shenao/q9oisul4oawbtyqg3p0vh6mf/Screen%20Shot%202019-12-10%20at%2012.11.51%20AM.png
  [3]: http://static.zybuluo.com/Shenao/udhc8ask94x8xsi8g3uve5ku/Screen%20Shot%202019-12-10%20at%201.53.04%20PM.png