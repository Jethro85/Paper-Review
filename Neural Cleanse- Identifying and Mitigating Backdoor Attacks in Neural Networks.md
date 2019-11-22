# Neural Cleanse: Identifying and Mitigating Backdoor Attacks in Neural Networks

标签（空格分隔）： Detection Mitigation  

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
In this paper, their key intuition lies in how to detect and identify backdoor.

![Screen Shot 2019-11-22 at 3.31.43 PM.png-257.3kB][1]

Backdoor triggers creat "shortcuts" from within regions of space belonging to a label into the region belonging to A. 


  [1]: http://static.zybuluo.com/Shenao/exbnpmklrwbsse5ndupdabr9/Screen%20Shot%202019-11-22%20at%203.31.43%20PM.png