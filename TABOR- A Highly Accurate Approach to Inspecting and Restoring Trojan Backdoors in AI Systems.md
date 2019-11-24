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


  [1]: http://static.zybuluo.com/Shenao/5v0y9fd6oh9yjd7bmjy8hh13/1.JPG
  [2]: http://static.zybuluo.com/Shenao/4ofhqd09ojtciu6c6s3i7uwk/Capture.JPG
  [3]: http://static.zybuluo.com/Shenao/033lsveq604cf0u0knhzdyxi/1.JPG
  [4]: http://static.zybuluo.com/Shenao/hb29mdhwyrof1uf7ppwu8nq4/1.JPG
  [5]: http://static.zybuluo.com/Shenao/o0tegfo6p0eyobx3fqbemesc/1.JPG
  [6]: http://static.zybuluo.com/Shenao/djqysab6t1xu1qhjg5c0pubc/1.JPG
  [7]: http://static.zybuluo.com/Shenao/q9c8jzxfgqas3yj3s9orssmc/1.JPG
  [8]: http://static.zybuluo.com/Shenao/jsgzftnjnnruj3oydo55apcv/1.JPG