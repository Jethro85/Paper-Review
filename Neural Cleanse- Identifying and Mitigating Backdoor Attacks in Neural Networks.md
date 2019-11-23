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

Backdoor triggers creat "shortcuts" from within regions of space belonging to a label into the region belonging to A. So detecting these 'shortcuts' makes the backdoor detection feasible.  

![Screen Shot 2019-11-22 at 3.31.43 PM.png-257.3kB][1]

Top figure shows a clean model, where more modification is needed to move samples of B and C across decision boundaries to be misclassified into label A.  

Bottom figure shows the infected model, where the backdoor changes decision boundaries and creates backdoor areas close to B and C. These backdoor areas reduce the amount of modification needed to misclassify samples of B and C into the target label A.  

So by measuring the minimum amount of perturbation necessary to change all inputs from each region to the target region, we can detect these shortcuts. But what is the smallest delta necessary to transform any input whose label is B or C to an input with label A?

![Screen Shot 2019-11-22 at 4.24.06 PM.png-90.4kB][2]

According to this observation, we know in a region with a trigger shortcut, no matter where an input lies in the space, the amount of perturbation needed to classify this input as A is bounded by the size of the trigger.

What's more, there is another observation:  

![Screen Shot 2019-11-22 at 4.29.18 PM.png-35.4kB][3]

So δ∀→t is significantly smaller than those required to transform any input to an uninfected label. Now we can detect a trigger Tt by detecting **an abnormally low value of δ∀→i** among all the output labels.

### 2. Detection and Identification Methodology

From the key intuition, we know in an infected model, it requires much smaller modifications to cause misclassification into the target label than into other uninfected labels. Therefore,
we iterate through all labels of the model, and determine if
any label requires significantly smaller amount of modification
to achieve misclassification into. 

Here, the amount of modification can be gotten by an optimization scheme -- Reverse Engineering.

The optimization has two objectives:

![Screen Shot 2019-11-22 at 4.55.06 PM.png-23kB][4]

For a given target label to be analyzed (yt), the first objective is to find a trigger (m, ∆) that would misclassify clean images into yt. The second objective is to find a “concise” trigger, meaning a trigger that only modifies a limited portion of the image. We measure the magnitude of the trigger by the L1 norm of the mask m. 

f(·) is the DNN’s prediction function. ℓ(·) is the loss
function measuring the error in classification, which is cross
entropy in our experiment. By using Adam optimizer to solve the above optimization, we can get the reverse triggers. 

### 3. Mitigation Methodology
In the paper, they describe two complementary techniques. First, they create a filter for adversarial input that identifies and rejects any input with the trigger. Second, they patch the DNN using two methods: neuron pruning, and unlearning, to make DNN nonresponsive against the detected backdoor triggers.

#### 3.1  Filter for Detecting Adversarial Inputs
They build a filter based on neuron activation profile for the reversed trigger. This is measured as the average neuron activations of the top 1% of neurons in the second to last layer. Given some input, the filter identifies potential adversarial inputs as those with activation profiles higher than a certain threshold. 

#### 3.2  Patching via Neuron Pruning
target the second to last layer, and prune neurons by order of highest rank first(i.e. prioritizing those that show biggest activation gap between clean and adversarial inputs).

#### 3.3  Patching via Unlearning
use the reversed trigger to train the infected DNN to recognize correct labels even when the trigger is present.   


## EXPERIMENTS
To evaluate against BadNets, they use four tasks and inject backdoor using their proposed technique: (1) Hand-written Digit Recognition (MNIST), (2) Traffic Sign Recognition (GTSRB), (3) Face Recognition with large number of labels (YouTube Face), and (4) Face Recognition using a complex model (PubFig). For Trojan Attack, they use two already infected Face Recognition models used in the original work and shared by authors, Trojan Square, and Trojan Watermark.

### Detection Performance 
Results show that all infected models have anomaly index larger than 3, indicating > 99.7% probability of being an infected model. This means that the methodology can detect an infected DNN.

At the same time, their approach can also determine which labels are infected.

### Mitigation Performance
Patching via neuron pruning works well on the backdoors mitigation of BadNets, but works bad on Trojan Models. The reason is that neuron activations do a poor job of matching the reverse engineered triggers and the originals, so pruning cannot work well. 

Patching via Unlearning works well on backdoors mitigation of Badnets and Trojan Models. In this approach, they use fine-tune for 1 epoch. Results show that after fine-tune with clean images, attack success rates of Trojan Attack models down to 10.91% and 0% for Trojan Square, and Trojan Watermark respectively. So Trojan Attack models, with their highly targeted re-tuning of specific neurons, are much more sensitive to unlearning.  A clean input that helps reset a few key neurons disables the attack. In contrast, BadNets injects backdoors by updating all layers using a poisoned dataset, and seems to require significantly more work to retrain and mitigate the backdoor.

  [1]: http://static.zybuluo.com/Shenao/exbnpmklrwbsse5ndupdabr9/Screen%20Shot%202019-11-22%20at%203.31.43%20PM.png
  [2]: http://static.zybuluo.com/Shenao/ytlpeyfgdi3k4548k2s2puet/Screen%20Shot%202019-11-22%20at%204.24.06%20PM.png
  [3]: http://static.zybuluo.com/Shenao/1estmywdkl80hkk8j2lpd7h6/Screen%20Shot%202019-11-22%20at%204.29.18%20PM.png
  [4]: http://static.zybuluo.com/Shenao/5m5naliziu7dj7iqic8kgzlq/Screen%20Shot%202019-11-22%20at%204.55.06%20PM.png