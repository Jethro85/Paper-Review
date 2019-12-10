#  Detecting Backdoor Attacks on Deep Neural Networks by Activation Clustering 

Label: detection, mitigation

---

Paper Link: https://arxiv.org/pdf/1811.03728.pdf

---

## SUMMARY
Detecting backdoor attack is challenging because the unexpected behavior occurs only when a backdoor trigger, which is known only to the adversary, is present. In this paper, In this paper, they propose the **Activation Clustering (AC)** method for detecting poisonous training samples crafted to insert backdoors into DNNs. This method analyzes the neural network activations of the training data to determine whether it has been poisoned, and, if so, which datapoints are poisonous.  

Their contributions are the following:

- They propose the first methodology for detecting poisonous data maliciously inserted into the training set to generate backdoors that does not require verified and trusted data.  
- We demonstrate that the AC method is highly successful at detecting poisonous data in different applications by evaluating it on three different text and image datasets.  
- We demonstrate that the AC method is robust to complex poisoning scenarios in which classes are multimodal (e.g. contain sub-populations) and multiple backdoors are inserted.

## MAIN IDEA
### 1. Intuition 
While backdoor and target samples receive the same classification by the poisoned network, the reason why they receive this classification is different.   

In the case of standard samples from the target class, the network identifies features in the input that it has learned correspond to the target class. In the case of backdoor samples, it identifies features associated with the source class and the backdoor trigger, which causes it to classify the input as the target class. This difference in mechanism should be evident in the network activations, which represent how the network made its “decisions”.  


The above figure shows activations of the last hidden neural network layer for clean and legitimate data projected onto their first three principle components. Figure a shows the activations of class 6 in the poisoned MNIST dataset, b shows the activations of the poisoned speed limit class in the LISA dataset, and c shows the activations of the poisoned negative class for Rotten Tomatoes movie reviews. In
each, it is easy to see that the activations of the poisonous and legitimate data break out into two distinct clusters. In contrast, Figure d displays the activations of the positive class, which was not targeted with poison. Here we see that the activations do not break out into two distinguishable clusters.  

### 2. Obtain the activations 


The Algorithm they use to obtain the activations are shown above. First, the neural network is trained using untrusted data that potentially includes poisonous samples. Then, the network is queried using the training data and the resulting activations of the last hidden layer are retained. *Analyzing the activations of the last hidden layer was enough to detect poison.*   

### 3. Separate and determine the activations 
Once the activations are obtained for each training sample, they are segmented according to their label and each segment clustered separately. To cluster the activations, they first **reshaped the activations into a 1D vector** and **then performed dimensionality reduction using Independent Component Analysis (ICA)**. Dimensionality reduction before clustering is necessary to avoid known issues with clustering on very high dimensional data. In particular, as dimensionality increases distance metrics in general (and specifically the Euclidean metric used here), are less effective at distinguishing near and far points in high dimensional spaces. This will be especially true when we have hundreds of thousands of activations. Reducing the dimensionality allows for more robust clustering, while still capturing the majority of variation in the data. 

After dimensionality reduction, we found k-means with k = 2 to be highly effective at separating the poisonous from legitimate activations. However, k-means will separate the activations into two clusters, regardless of whether poison is present or not. Thus, we still need to determine which, if any, of the clusters corresponds to poisonous data. In this paper, they show three methods to solve this: Exclusionary Reclassification, Relative Size Comparison and Silhouette Score.

### 4. Summarizing Clusters
User may want to verify that the data is indeed poisoned before taking action. So we need methods that summarize the contents of each cluster so that the user can quickly analyze and determine if a given cluster is poisonous and, if so, what the correct label is.

For image datasets, they propose constructing image sprites for each cluster and averaging the images for the activations in the cluster.
  
For text datasets, they summarized each cluster using Latent Dirichlet allocation (LDA) to identify the primary topics in each cluster. 

## EXPERIMENTS

They did experiments on the MNIST, LISA, and Rotten Tomatoes datasets. After each of these datasets was poisoned in the manner described in the Case Studies section, a neural network was trained to perform classification and tested to ensure that the backdoor was successfully inserted. Next, AC was applied using the following setup. First, the activations were projected onto 10 independent components. Then, 2-means was used to perform clustering on the reduced activations. Lastly, they used exclusionary reclassification to determine which cluster, if any, was poisoned.

The results of backdoor detection are shown below: 


From the results of MNIST experiments with 10% of the data poisoned, we can see that both accuracy and F1 score are nearly 100% for every class. Nearly identical results were obtained for 15% and 33% poisoned data. In contrast, clustering the raw inputs on 10% poisoned data was only able to achieve a 58.6% accuracy and a 15.8% F1 score when 10% of each class was poisoned. When 33% of each class was poisoned, clustering the raw data performed better with a 90.8% accuracy and an 86.38% F1 score but was still not on par with AC’s near perfect detection rates.  
On the LISA data set, they achieved 100% accuracy and an F1 score of 100% in detecting poisonous samples where 33% and 15% of the stop sign class was poisoned. For our text-based experiments, they also achieved 100% accuracy and F1 score in detecting poisonous samples in a data set where 10% of the negative class was poisoned.

(They also do experiments about Multimodal Classes and Poison and analyze the Cluster. Results show that their method is robust to multimodal classes and complex poisoning schemes. )


## DISCUSSION
Detecting the backdoor according to activations is a good idea. Through a variety of experiments on two image and one text datasets, they demonstrate the effectiveness of the AC method at detecting and repairing backdoors.

However, their repair method is based on retrain. It's true retraining using training data can mitigate the backdoor. But for a larger dataset, retraining for a large epoches is not efficient. 