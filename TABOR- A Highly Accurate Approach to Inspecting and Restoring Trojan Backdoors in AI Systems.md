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





