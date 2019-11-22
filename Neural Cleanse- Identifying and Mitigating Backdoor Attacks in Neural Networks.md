# Neural Cleanse: Identifying and Mitigating Backdoor Attacks in Neural Networks

Label: Detection, Mitigation  

---
paper: https://sites.cs.ucsb.edu/~bolunwang/assets/docs/backdoor-sp19.pdf  
code: https://github.com/bolunwang/backdoor

## SUMMARY

In this paper, they make the following contributions to the defense
against backdoors in neural networks:
 - They propose a novel technique for **detecting and reverse engineering hidden triggers** embedded inside deep neural networks.
 - They implement and validate technique on a variety of neural network applications, including handwritten digit recognition, traffic sign recognition, facial recognition with large number of labels, and facial recognition using transfer learning. They reproduce BadNets and Trojan attack and use them in experiments.
 - They develop and validate via detailed experiments three methods of mitigation:   
i) an early filter for adversarial inputs that identifies inputs with a known trigger  
ii) a model patching algorithm based on neuron pruning  
iii) a model patching algorithm based on unlearning.

## MAIN IDEA
### 1. key intuition
In this paper, the key intuition lies in how to detect and identify backdoor.  

Backdoor triggers create "shortcuts" from within regions of space belonging to a label into the region belonging to A. So detecting these 'shortcuts' makes the backdoor detection feasible.  

![Screen Shot 2019-11-22 at 3.31.43 PM.png-257.3kB = 50*100][1]

Top figure shows a clean model, where more modification is needed to move samples of B and C across decision boundaries to be misclassified into label A.  

Bottom figure shows the infected model, where the backdoor changes decision boundaries and creates backdoor areas close to B and C. These backdoor areas reduce the amount of modification needed to misclassify samples of B and C into the target label A.  

So by measuring the minimum amount of perturbation necessary to change all inputs from each region to the target region, we can detect these shortcuts. But what is the smallest delta necessary to transform any input whose label is B or C to an input with label A?

![Screen Shot 2019-11-22 at 4.24.06 PM.png-90.4kB = 10*10][2]

According to this observation, we know in a region with a trigger shortcut, no matter where an input lies in the space, the amount of perturbation needed to classify this input as A is bounded by the size of the trigger.

What's more, there is another observation:  

![Screen Shot 2019-11-22 at 4.29.18 PM.png-35.4kB][3]

So δ∀→t is significantly smaller than those required to transform any input to an uninfected label. Now we can detect a trigger Tt by detecting **an abnormally low value of δ∀→i** among all the output labels.

### 2. Detection and Indetification Methodology


  [1]: http://static.zybuluo.com/Shenao/exbnpmklrwbsse5ndupdabr9/Screen%20Shot%202019-11-22%20at%203.31.43%20PM.png
  [2]: http://static.zybuluo.com/Shenao/ytlpeyfgdi3k4548k2s2puet/Screen%20Shot%202019-11-22%20at%204.24.06%20PM.png
  [3]: http://static.zybuluo.com/Shenao/1estmywdkl80hkk8j2lpd7h6/Screen%20Shot%202019-11-22%20at%204.29.18%20PM.png