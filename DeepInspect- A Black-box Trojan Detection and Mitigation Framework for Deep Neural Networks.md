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

DeepInspect (DI) consists of three main steps: model inversion to recover a substitution training dataset, trigger reconstruction using a conditional Generative Adversarial Network (cGAN), and anomaly detection based on statistical hypothesis testing. 

### 2. Model Inversion 
They employ model inversion to recover a substitution training set {XMI , YMI } which assists generator training in the next step.

**Data samples can be extracted from a pre-trained model and formulates model inversion as an optimization problem.** The objective function of MI is shown below, which is iteratively minimized via gradient descent.  

c(x) = 1 − f(x;t) + AuxInfo(x).  

Here, x is the input data, t is the target class to recover, f is the probability that the queried model predicts t given x as its input, AuxInfo(x) is an optional term incorporating auxiliary constraints on the input.


### 3. Trigger Generation
The key idea of DeepInspect is to train a conditional generator that learns the probability density distribution (pdf) of the Trojan trigger whose perturbation level serves as the detection statistics. Particularly, DI employs cGAN to ‘emulate’ the process of the Trojan attack. The objective of cGAN training is described in Eq. (2). Here, D is the queried DNN, t is the examined attack target, x is a sample from the approximate data distribution px obtained by MI, and the trigger is the output of the conditional generator ∆ = G(z, t). sampled from trigger distribution p∆.  

D(x + G(z, t)) = t. (2)  



The above picture shows the high-level overview of our trigger generator. 

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





