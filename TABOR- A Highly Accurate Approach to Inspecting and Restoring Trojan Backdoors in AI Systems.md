# TABOR: A Highly Accurate Approach to Inspecting and Restoring Trojan Backdoors in AI Systems

label: detection

---

paper: https://arxiv.org/pdf/1908.01763.pdf

## SUMMARY

Neural Cleanse is the most recent research work that can perform trojan backdoor inspection without the assumption of having access to the training database. It defines an objective function and formalizes trojan detection as a non-convex optimization problem. It demonstrates decent performance in pointing out the existence of a trojan backdoor.   

However, Neural Cleanse becomes completely futile, especially when an infected model ingests a trigger with varying size,shape, and location. This is because these attributes of an ingested trigger significantly vary the number of adversarial samples in the adversarial subspace, which forces the adversarial sample search into encountering more adversarial samples that are not the true interest.  

Inspired by the finding and the analysis above, they propose a new trojan detection approachandname it after TABOR. Similar to Neural Cleanse, TABOR also formulates trojan detection as an optimization problem and thus views the detection as searching trigger - inserted inputs in an adversarial subspace.

In summary, this paper makes the following contributions: 
- They evaluate the state-of-the-art trojan backdoor inspection approach Neural Cleanse, demonstrating that it almost completely fails when the size, shape, and location of a trigger pertaining to the trojan vary. 
- They design and develop TABOR, **a new trojan backdoor detection approach that utilizes optimization regularization inspired by explainable AI techniques** as well as heuristics, and **a new trigger quality measure** to reduce the false alarms introjandetection. 
- They insert backdoors–with various sizes, in different shapes and at different locations–into various DNN models trained on different datasets and then use the infected models to extensively evaluate TABOR. In terms of detection accuracy and fidelity of the restored triggers, their comparison results show TABOR is much better than Neural Cleanse, a state of-the-art technique. 
- They evaluate TABOR by varying trojan insertion techniques, model complexity and hyperparameters. The results show that TABOR is robust to changes of these factors, indicating the ease of deployment of TABOR inreal-world systems.

## MAIN IDEA

A trojan detection technique has to minimize the negative influence of false alarms as well as incorrect triggers. So first, they conduct an empirical study to analyze the false alarms as well as incorrect triggers. After experiments, they find the false alarms and incorrect triggers have several common characteristics: Some of their presentations are either extremely scattered or overly large; Some of them Block the Key Object; Some of the resolved trigger overlay the trigger intentionally inserted but presents themself in a larger size. Then they design wheir objective function under the guidance of these characteristics. 

### 1. Regularization term for overly large triggers
They define the regularization term pertaining to the penalization of overly large triggers as follows:  
![1.JPG-21.1kB][1]
As we can observe in this equation, M represents the mask of a trigger and ∆′ indicates the color pattern outside the region of that trigger. Imposing an elastic net on both of these terms and then summing them together, we can obtain a measure indicating the total amount of non-zero elements in M and ∆′. For an overly large trigger, the value of R1(M,∆) is relatively large. Therefore, by adding this regularization term into the Equation(1), we can easily remove those triggers overly large and thus shrink the amount of adversarial samples in adversarial subspace. 

### 2. Regularization term for scattered triggers
They design the regularization term pertaining to the penalization of scattered triggers as follows:  
![Capture.JPG-23.5kB][2]
s(·) is a smoothness measure. Applying it to M and ∆′, it describes the density of zero and non-zero elements. It is not difficult to imagine, the lower this smoothness measure is,the less scattered the resolved triggers will be. 

### 3. Regularization term for blocking triggers
They design the regularization term pertaining to the blocking triggers as follows:  
![1.JPG-14.1kB][3]
x⊙(1−M) represents an image from which we crop the corresponding trigger. yt′ stands for the true class of x. As we can see from the equation above, this regularization term R3 introduces a loss function L(·) to measure the similarity between the true class and the prediction forx⊙(1−M). 

### 4. Regularization term for overlaying triggers
In this part, they learn from the idea of explainable AI technique. The explainable AI technique could pinpoint the top important features that contribute to the prediction result by solving the following function:
![1.JPG-14.8kB][4]
Combing with the formula of x: 
![1.JPG-17.5kB][5]
We can get:
![1.JPG-29.8kB][6]
If we make M1=M, eventually we can obtain a simplified form for the regularization term R4: 
![1.JPG-13.6kB][7]

### 5. Metric Design
Similar to the optimization problem alone defined in the Equation(1), for any given value yt and a deep neural network model f(·), we still inevitably obtain a local optimum. In Neural Cleanse, researchers have computed the L1 norm for each local optimum and then utilized an outlier detection method (MAD) to filter out local optima indicating incorrect triggers and false alarms. However, L1 norm provides the outlier algorithm with extremely poor distinguish ability. In this work, they therefore use the MAD outlier detection method to eliminate false alarms and incorrect triggers but replace the L1 norm with a new metric:
![1.JPG-27.1kB][8]

acc(att) indicates the misclassification rate, observed when we insert that resolved trigger into a set of clean images and then feed these contaminated images into the learning model f(·). acc(exp) represents the classification accuracy, observed when we feed the explanation(i.e.,important features alone) of the contaminated images into f(·). acc(crop) denotes the classification accuracy, observed when we crop resolved triggers from the corresponding contaminated images and then input the cropped images to f(·). For the first two terms in the equation above, they represent the sparsity measure indicating non-zero elements in the trigger and the smoothness measure of the trigger, respectively.

## EXPERIMENTS
They evaluate the effectiveness of the proposed technique by answering the following questions.  
(1) Does the effectiveness of TABOR vary when the size, position, and shape of trojan backdoorschange?   
(2) Does the dimensionality of the input space tied to a target model influence the effectiveness of TABOR?   
(3) How effectively does TABOR demonstrate when there are multiple triggers inserted into target models?  
(4) Does the complexity of the target model influence the effect of trojan backdoor detection?   
(5) How sensitive is TABOR to the hyperparameters?   
(6) How effectiveis TABOR for different methods ofinserting a trojan backdoor?   
(7) In comparison with the state-of-the-art technique – Neural Cleanse, how well does TABOR perform trojan detection and trigger restoration under the settings above?

The **main results** are shown in the following picture:
![1.JPG-464.9kB][9]

For learning models that enclose a trojan backdoor with a different size, in a different shape or at a different location, Neural Cleanse oftentimes cannot point out the trojan existence. Even if it sometimes points out trojan existence, in one of the cases, it fails to pinpoint the trigger truly inserted. For a clean model carrying no trojan backdoor, Neural Cleanse mistakenly reports the model is implanted with a trojan backdoor. 

There are two reasons:  

1. In different adversarial subspaces, the total number of adversarial samples might vary, influencing the stability of Neural Cleanse in trojan detection.

2.  regardless of whether a backdoor exists in a learning model, the optimization-based technique could always find a local optimal solution for a corresponding optimization problem and thus deem it as a reasonably good local optimum. 

In comparison with the extremely poor performance Neural Cleanse exhibits in the setting of trigger shape/size/location variations, **TABOR demonstrates a significant improvement in trojan backdoor detection.** 

The performance of TABOR from the fidelity aspect is also better. As is illustrated in Table 1, overall, TABOR demonstrates higher measures in precision, recall, and F-score than Neural Cleanse. This indicates **TABOR could restore a trojan backdoor with higher fidelity.**

What's more, from the table we can see they perform trojan detection on learning models taking input in different dimensionalities (ImageNet with 224×224 and GTSRB with 32×32), for both Neural Cleanse and TABOR, **the difference in input dimensionality has less impact upon their backdoor detection**.


  [1]: http://static.zybuluo.com/Shenao/5v0y9fd6oh9yjd7bmjy8hh13/1.JPG
  [2]: http://static.zybuluo.com/Shenao/4ofhqd09ojtciu6c6s3i7uwk/Capture.JPG
  [3]: http://static.zybuluo.com/Shenao/033lsveq604cf0u0knhzdyxi/1.JPG
  [4]: http://static.zybuluo.com/Shenao/hb29mdhwyrof1uf7ppwu8nq4/1.JPG
  [5]: http://static.zybuluo.com/Shenao/o0tegfo6p0eyobx3fqbemesc/1.JPG
  [6]: http://static.zybuluo.com/Shenao/djqysab6t1xu1qhjg5c0pubc/1.JPG
  [7]: http://static.zybuluo.com/Shenao/q9c8jzxfgqas3yj3s9orssmc/1.JPG
  [8]: http://static.zybuluo.com/Shenao/jsgzftnjnnruj3oydo55apcv/1.JPG
  [9]: http://static.zybuluo.com/Shenao/2quiyl45j7eji0h5pij2e757/1.JPG