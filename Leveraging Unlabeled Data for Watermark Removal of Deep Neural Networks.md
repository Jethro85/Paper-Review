# Leveraging Unlabeled Data for Watermark Removal of Deep Neural Networks

Label: Watermark Removal  
Conference: ICML 2019

---
Paper Link: https://ruoxijia.github.io/assets/papers/watermark_removal_icml19_workshop.pdf

## SUMMARY  
Recent work has explored different watermarking techniques to protect the pre-trained deep neural networks from potential copyright infringements. Although several existing techniques could effectively embed such watermarks into the DNNs, they could be vulnerable to adversaries who aim at removing the watermarks.  

In this work, they study the effectiveness of **fine-tuning based watermark removal techniques**. To effectively remove the watermarks while preserving the model performance, finetuning based approaches often require the adversary to have **a sufficient amount of labeled data** drawn from the same distribution as the data used for evaluation. While a large amount of labeled data could be expensive to collect, unlabeled data is much cheaper to obtain.In this work, they **propose to utilize the pre-trained model to annotate the unlabeled samples,** and augment the fine-tuning training data with them.  

## MAIN IDEA  
In this work, they focus on fine-tuning based approaches for watermark removal. So they first propose an adaption of existing fine-tuning techniques to improve the efficacy of watermark removal, referred to as Basic FineTuning (FT-Basic). In cases where the adversary has limited fine-tuning data, they further propose data augmentation with unlabeled data, referred to as Fine-tuning with Augmentation of Unlabeled data (FTAU)  

### 1. Basic Fine-tuning (FT-Basic)  
some prior work show that existing watermarking techniques are robust against fine-tuning based techniques, even if the adversary fine-tunes the entire model and has access to the same benign data as the owner. They suppose that the key reason may be that the learning rate for fine-tuning is too small to change the model weights. So **they simply increase the initial learning rate for fine-tuning the entire model and properly decaying the learning rate**, the adversary is able to remove the watermarks without compromising the model performance on his task.   

### 2. Fine-tuning with Augmentation of Unlabeled data (FTAU)
Generally the adversary does not have comparable amount of training data to the model owner, and by fine-tuning with only the limited labeled data, removing watermarks may cause large degradation of model accuracy on his task.  

So they propose to augment the fine-tuning data with unlabeled samples, which could easily be collected from the Internet. Let U be the unlabeled sample set. Then they **use the pre-trained model as the labeling tool** to label the unlabeled sample set, i.e., yu = fθ(xu) for each xu ∈ U. They will show that leveraging unlabeled data significantly decreases the in-distribution labeled samples needed for effective watermark removal.  

## EXPERIMENTS  
They evaluate their fine-tuning approaches on CIFAR-10, CIFAR-100 and STL-10. They evaluate pattern-based techniques and instance-based techniques. They evaluate both FT-Basic and FTAU. They also compare with a baseline method that trains the entire model from scratch without leveraging the pre-trained model, denoted as FS.  

The initial learning rate for finetuning is 0.001 which is 100× smaller than the initial learning rate for pre-training (0.1). For all experiments, they set the initial fine-tuning learning rate to be 0.05, and decay it by 0.9 after every 500 timesteps.   

**They consider the watermarks are removed when the watermark accuracy is below 20% for models pre-trained on CIFAR-10, and below 10% for models trained on CIFAR-100.**  

### 1. Transfer Learning  
The labeled part of STL-10 only includes 5,000 samples, which is insufficient for training a model with a high accuracy. Therefore, they perform the transfer learning to adapt from a pre-trained CIFAR-10 model to STL-10. They finetune the pre-trained model with a learning rate of 0.001, so that its watermark accuracy remains above 70%. 

After getting the transferred model, they do watermark removal using FT-Basic and FTAU and compare results with FS:  

![Screen Shot 2019-11-26 at 5.13.19 PM.png-36kB][1]

Results show that with the FT-Basic method, removing watermarks already does not affect the model performance on the testset, and using FTAU further improves the prediction accuracy.

### 2. Watermark Removal in the non-transfer learning setting for pattern-based techniques

They conduct experiments on both CIFAR-10 and CIFAR-100. The unlabeled data is obtained from the unlabeled part of STL-10, which includes 100,000 samples. 

![Screen Shot 2019-11-26 at 5.17.50 PM.png-112.2kB][2]

Results show that when the adversary has 80% of the entire training set, using FT-Basic already achieves a higher test accuracy than the pre-trained model, while removing the watermarks. Note that the watermark accuracies are still above 95% using the fine-tuning approaches in previous work, suggesting the effectiveness of modification of learning rate. However, when the adversary only has a small proportion of labeled training set, the test accuracy could degrade. Although the test accuracy only drops for about 1% on CIFAR10 even if the adversary has only 20% of the entire training set, the accuracy decrease could be up to 5% on CIFAR-100.

By leveraging the unlabeled data, the adversary is able to achieve the same level of test performance as the pre-trained model with only 20% ∼ 30% of the entire training set. Furthermore, FTAU enables the adversary to fine-tune the model without any labeled training data, and by solely relying on the unlabeled data, the accuracy of the fine-tuned model achieves within 1% difference from the pre-trained model on CIFAR-10, and the accuracy of fine-tuned CIFAR-100 model nearly reaches the performance of the model trained with 80% CIFAR-100 data from scratch. Note that the unlabeled STL-10 data samples are drawn from very different distribution; in particular, the label sets of CIFAR100 and STL-10 barely overlap. These results show that their approach is effective without the requirement that the unlabeled data comes from the same distribution, which makes it a simple watermark removal for the adversary, thus poses threats to the robustness of pattern-based watermarks.

### 3. Watermark Removal in the non-transfer learning setting for instance-based techniques

![Screen Shot 2019-11-26 at 5.23.09 PM.png-109.4kB][3]

Compared to pattern-based techniques, removing instance-based watermarks could result in a larger decrease of test accuracy, indicating that while pattern-based watermarks are more often used, they are easier to remove than instance-based watermarks. However, still, FT-Basic method is more effective than previous fine-tuning attempts, and FTAU enables the adversary to obtain a model with high accuracy despite having limited labeled data.

## MY OPINION  

This paper is mainly about the Watermark Removal, so from my perspective, they should show more results about 'Removal' instead of results of test accuracy after removal. 

They only mention the results about Watermark Removal one time: 'We consider the watermarks are removed when the watermark accuracy is below 20% for models pre-trained on CIFAR-10, and below 10% for models trained on CIFAR-100'. They didn't compare the removal results of their method with other fine-tune methods, which uses higher learning rate. This is a shortcoming of this paper. 

  [1]: http://static.zybuluo.com/Shenao/bq1rf7jqct8om34pwxwga07g/Screen%20Shot%202019-11-26%20at%205.13.19%20PM.png
  [2]: http://static.zybuluo.com/Shenao/jygu2lmprh9g0dxu7i0cfgia/Screen%20Shot%202019-11-26%20at%205.17.50%20PM.png
  [3]: http://static.zybuluo.com/Shenao/gy3ohk6q7enu6lax4c9x8ou1/Screen%20Shot%202019-11-26%20at%205.23.09%20PM.png