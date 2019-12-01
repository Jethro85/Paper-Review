# Turning Your Weakness Into a Strength: Watermarking Deep Neural Networks by Backdooring 

label: watermarking  
conference: usenix 2018

---
Paper Link: https://arxiv.org/pdf/1802.04633.pdf    
github link: https://github.com/adiyoss/WatermarkNN (pytorch)  
blog: https://medium.com/@carstenbaum/the-ubiquity-of-machine-learning-and-its-challenges-to-intellectual-property-dc38e7d66b05  

## SUMMARY  
Selling pre-trained DNN models can be a lucrative business model. Unfortunately, once the models are sold they can be easily copied and redistributed. To avoid this, a tracking mechanism to identify models as the intellectual property of a particular vendor is necessary.   
 
In this work, they present an approachfor watermarking Deep Neural Networks in a black-box way. First, they propose a simple and effective technique for watermarking Deep Neural Networks. They provide extensive empirical evidence using state-of-the-art models on well-established benchmarks, and demonstrate the robustness of the method to various nuisance including adversarial modiﬁcation aimed at removing the watermark. Second, they present a cryptographic modeling of the tasks of watermarking and backdooring of Deep Neural Networks, and show that the former can be constructed from the latter (using a cryptographic primitive called commitments) in a black-box way.   

## MAID IDEA  
### 1. Deﬁnitions and Models
In this section, the authors give the definitions and models. For definition of Machine Learning and Backdoors in Neural Networks, they are easy to understand. Backdooring neural networks is a technique to deliberately train a machine learning model to output wrong (when compared with the ground-truth function f) labels TL for certain inputs T.  

Then they give the defination of strong backdoors. Strong backdoors should have two properties: 1. Multiple Trigger Sets. For each trigger set that SampleBackdoor returns as part of a backdoor, it has minimal size n. Moreover, for two random backdoors, their trigger sets almost never intersect. 2. Persistency. With persistency, it is hard to remove a backdoor, unless one has knowledge of the trigger set T. 

Finaly they give the definition of Commitment schemes. Commitment schemes are a well knowncryptographic primitive which allows a sender to lock a secret x into a cryptographic leakage-free and tamper-proof vault and give it to some one else, called a receiver. It is neither possible for the receiver to open this vault without the help of the sender (**this is called hiding**), nor for the sender to exchange the locked secret to something else once it has been given away (**the binding property**). Formally, a commitment scheme consists of two algorithms: Com and Open. 

### 2. Deﬁning Watermarking  
They split a watermarking scheme into three algorithms: (i) a ﬁrst algorithm to generate the secret marking key mk which is embedded as the watermark, and the public veriﬁcation key vk used to detect the watermark later; (ii) an algorithm to embed the watermark into a model; and (iii) a third algorithm to verify if a watermark is present in a model or not. Formally, a watermarking scheme is deﬁned by the three PPT algorithms (KeyGen, Mark, Verify):  

![Capture.JPG-47.4kB][1]

In terms of security, a watermarking scheme must be functionality-preserving, provide unremovability, unforgeability and enforce non-trivial ownership: (They give the definition of these four properities respectively)

- Functionality-preserving: We say that a scheme is functionality-preserving if a model with a watermark is as accurate as a model without it. 
- Non-trivial ownership: Non-trivial ownership means that even an attacker which knows watermarking algorithm is not able to generate in advance a key pair (mk,vk) that allows him to claim ownership of arbitrary models that are unknown to him.
- Unremovability: Unremovability denotes the property that an adversary is unable to remove a watermark, even if he knows about the existence of a watermark and knows the algorithm that was used in the process. 
- Unforgeability: Unforgeability means that an adversary that knows the veriﬁcation key vk, but does not know the key mk, will be unable to convince a third party that he (the adversary) owns the model.

### 3. Watermarking From Backdooring  
This section gives a theoretical construction of privately veriﬁable watermarking based on any strong backdooring and a commitment scheme. On a high level, the algorithm ﬁrst embeds a backdoor into the model; this backdoor itself is the marking key, while a commitment to it serves as the veriﬁcation key.  

More concretely, let (Train,Classify) be an ε accurate ML algorithm, Backdoorbe a strong backdooring algorithm and (Com,Open) be a statistically hiding commitment scheme. Then deﬁne the three algorithms (KeyGen, Mark, Verify) respectively.   

They then prove the properties respectively: Correctness; Functionality-preserving; Unremovability; Unforgeability; Non-trivial ownership. **(For the demonstration of Unremovability, Unforgeability and Non-trivial ownership, they are too mathematical, so I didn't understand well)**  

## EXPERIMENTS
In experiments, they use CIFAR-10, CIFAR-100 and ImageNet as datasets. The images on which they train the model to deliberately misclassify (Trigger Set) were chosen as random images which were unrelated to the data from the dataset. For each such Trigger Set image, a class was randomly assigned during the watermarking process. 

**During training, for each batch, denote as bt the batch at iteration t, they sample k trigger set images and append them to bt.** (This is the constitute of training data)  

![1_3nES2F0HHlBThbMRcvm8xw.png-22.7kB][2]

It can be seen in the table above that the Trigger Set images were always classified as trained, i.e. that the watermark is correctly placed. At the same time, the change in the testing accuracy with or without the watermark is negligible.  

![1_xZZ7NL8jsIyuZu_vQoU33Q.png-41kB][3]

Furthermore, they ran experiments that attempt to identify and remove the watermark. The largest class of attacks can be subsumed as Fine-Tuning, where one attempts to alter a model such as to train it to a different but related task. Here, the results suggest that while both models reach almost the same accuracy on the test set, the FROM-SCRATCH models are superior or equal to the PRE-TRAINED models overall ﬁne-tuning methods. FROM-SCRATCH reaches roughly the same accuracy on the trigger set when each of the four types of ﬁne-tuning approaches is applied.   

Notice that this observation holds for both the CIFAR-10 and CIFAR-100 datasets, where **for CIFAR-100 it appears to be easier to removethe trigger set using the PRE-TRAINED models**.


  [1]: http://static.zybuluo.com/Shenao/yy1754hhsmcos32iamtvz0rb/Capture.JPG
  [2]: http://static.zybuluo.com/Shenao/zm9z2vwlzfeyb4ubprlmob6p/1_3nES2F0HHlBThbMRcvm8xw.png
  [3]: http://static.zybuluo.com/Shenao/ol33tfbwitmfkhelsz5srw03/1_xZZ7NL8jsIyuZu_vQoU33Q.png