# Latent Backdoor Attacks on Deep Neural Networks

label: backdoor attack, transfer learning 

---
Paper Link: http://people.cs.uchicago.edu/~ravenben/publications/pdf/pbackdoor-ccs19.pdf

## MAIN IDEA
Recent work proposed the concept of backdoor attacks on deep neural networks (DNNs), where misclassification rules are hidden inside normal models, only to be triggered by very specific inputs. However, these “traditional” backdoors assume a context where users **train their own models from scratch**, which rarely occurs in practice. 

Instead, users typically customize “Teacher” models already pretrained by providers through **transfer learning**. This customization process introduces significant changes to models and disrupts hidden backdoors, greatly reducing the actual impact of backdoors in practice.

In this paper, they describe **latent backdoors** that functions under transfer learning. Latent backdoors are incomplete backdoors embedded into a “Teacher” model, and automatically inherited by multiple “Student” models through transfer learning. If any Student models include the label targeted by the backdoor, then its customization process completes the backdoor and makes it active. 

Their contributions are as follows: 
- Proposing the latent backdoor attack and describe its components in detail on both the teacher and student sides.  
- Validating the effectiveness of latent backdoors using different parameters in a variety of application contexts in the image domain.  
- Validating and demonstrating the effectiveness of latent backdoors using 3 **real-world tests** on models, using physical data and realistic constraints.  
- Proposing and evaluating 4 potential defenses against latent backdoors. 




