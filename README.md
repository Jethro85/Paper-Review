# Paper-Review
This repo contains notes and short summaries of some DNN security and Software Engineering & AI related papers I come across.



## Attacks

### Backdoor Attack

- [x] Latent Backdoor Attacks on Deep Neural Networks (CCS 2019): [[Paper](http://people.cs.uchicago.edu/~ravenben/publications/pdf/pbackdoor-ccs19.pdf)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Latent%20Backdoor%20Attacks%20on%20Deep%20Neural%20Networks.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/ccs/YaoLZZ19)]



### Watermarking

- [x] Leveraging Unlabeled Data for Watermark Removal of Deep Neural Networks (ICML 2019): [[Paper](https://ruoxijia.github.io/assets/papers/watermark_removal_icml19_workshop.pdf)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Leveraging%20Unlabeled%20Data%20for%20Watermark%20Removal%20of%20Deep%20Neural%20Networks.md)]
- [x] Turning Your Weakness Into a Strength: Watermarking Deep Neural Networks by Backdooring (usenix 2018): [[Paper](https://arxiv.org/pdf/1802.04633.pdf)] [[Code](https://github.com/adiyoss/WatermarkNN)] [[Blog]( https://medium.com/@carstenbaum/the-ubiquity-of-machine-learning-and-its-challenges-to-intellectual-property-dc38e7d66b05)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Turning%20Your%20Weakness%20Into%20a%20Strength-%20Watermarking%20Deep%20Neural%20Networks%20by%20Backdooring.md)]  [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/uss/AdiBCPK18)]



### Textual Adversarial Attack

#### Char-level Attack

- [ ] TEXTBUGGER: Generating Adversarial Text Against Real-world Applications (NDSS 2019): [[Paper](https://arxiv.org/pdf/1812.05271.pdf)]
- [ ] Text processing like humans do: Visually attacking and shielding NLP systems (NAACL 2019): [[Paper](https://arxiv.org/pdf/1903.11508.pdf)]
- [ ] Black-box generation of adversarial text sequences to evade deep learning classifiers (S&P 2018): [[Paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8424632)] 
- [ ] Hotflip: White-box adversarial examples for text classification (ACL 2018): [[Paper](https://arxiv.org/pdf/1712.06751.pdf)]



#### Word-level Attack

- [ ] Generating natural language adversarial examples (EMNLP 2018): [[Paper](https://arxiv.org/pdf/1804.07998.pdf)]
- [ ] Deep text classification can be fooled (IJCAI 2018): [[Paper](https://arxiv.org/ftp/arxiv/papers/1704/1704.08006.pdf)]
- [ ] Generating Fluent Adversarial Examples for Natural Languages (ACL 2019): [[Paper](https://www.aclweb.org/anthology/P19-1559.pdf)]
- [ ] Generating natural language adversarial examples through probability weighted word saliency (ACL 2019): [[Paper](https://www.aclweb.org/anthology/P19-1103.pdf)]



## Backdoor Detection and Mitigation

- [x] Neural Cleanse: Identifying and Mitigating Backdoor Attacks in Neural Networks (S&P 2019): [[Paper](https://sites.cs.ucsb.edu/~bolunwang/assets/docs/backdoor-sp19.pdf)] [[Code](https://github.com/bolunwang/backdoor)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Neural%20Cleanse-%20Identifying%20and%20Mitigating%20Backdoor%20Attacks%20in%20Neural%20Networks.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/sp/WangYSLVZZ19)]

- [x] STRIP: A Defence Against Trojan Attacks on Deep Neural Networks (2019 ACSAC): 
[[Paper](https://arxiv.org/pdf/1902.06531.pdf)] [[Code](https://github.com/garrisongys/STRIP)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/STRIP-%20A%20Defence%20Against%20Trojan%20Attacks%20on%20Deep%20Neural%20Networks.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/acsac/GaoXW0RN19)]

- [x] Design and Evaluation of a Multi-Domain Trojan Detection Method on Deep Neural Networks (2019): [[Paper](https://arxiv.org/pdf/1911.10312.pdf)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Design%20and%20Evaluation%20of%20a%20Multi-Domain%20Trojan%20Detection%20Method%20on%20Deep%20Neural%20Networks.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/journals/corr/abs-1911-10312)]

- [x] DeepInspect: A Black-box Trojan Detection and Mitigation Framework for Deep Neural Networks (IJCAI 2019):
[[Paper](https://cseweb.ucsd.edu/~jzhao/files/DeepInspect-IJCAI2019.pdf)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/DeepInspect-%20A%20Black-box%20Trojan%20Detection%20and%20Mitigation%20Framework%20for%20Deep%20Neural%20Networks.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/ijcai/ChenFZK19)]

- [ ] ABS: Scanning Neural Networks for Back-doors by Artificial Brain Stimulation (CCS 2019): [[Paper](https://www.cs.purdue.edu/homes/taog/docs/CCS19.pdf)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/ccs/LiuLTMAZ19)]

- [x] TABOR: A Highly Accurate Approach to Inspecting and Restoring Trojan Backdoors in AI Systems (2019): 
[[Paper](https://arxiv.org/pdf/1908.01763.pdf)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/TABOR-%20A%20Highly%20Accurate%20Approach%20to%20Inspecting%20and%20Restoring%20Trojan%20Backdoors%20in%20AI%20Systems.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/journals/corr/abs-1908-01763)]

- [x] Detecting Backdoor Attacks on Deep Neural Networks by Activation Clustering (2019 AAAI): [[Paper](https://arxiv.org/pdf/1811.03728.pdf)] [[Code](https://github.com/IBM/adversarial-robustness-toolbox/blob/master/examples/mnist_poison_detection.py)] [[Notes](https://github.com/Jethro85/Paper-Review/blob/master/Detecting%20Backdoor%20Attacks%20on%20Deep%20Neural%20Networks%20by%20Activation%20Clustering.md)] [[Citation](https://dblp.uni-trier.de/rec/bibtex/conf/aaai/ChenCBLELMS19)]





## Software Engineering and AI

### DNN Testing

- [ ] DEEPSEC: deciding equivalence properties in security protocols theory and practice (S&P 2018): [[Paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8418623)]
- [ ] Guiding deep learning system testing using surprise adequacy (ICSE 2019): [[Paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=8812069)]
- [ ] DeepGauge: Multi-granularity testing criteria for deep learning systems (ASE 2018): [[Paper](https://dl.acm.org/doi/pdf/10.1145/3238147.3238202)]
- [ ] DeepXplore: Automated whitebox testing of deep learning systems (SOSP 2017): [[Paper](https://dl.acm.org/doi/pdf/10.1145/3132747.3132785)]
- [ ] DeepTest: Automated testing of deep-neural-network-driven autonomous cars (ICSE 2018): [[Paper](https://dl.acm.org/doi/pdf/10.1145/3180155.3180220)]
- [ ] DeepHunter: a coverage-guided fuzz testing framework for deep neural networks (ISSTA 2019): [[Paper](https://dl.acm.org/doi/pdf/10.1145/3293882.3330579)]
- [ ] There is limited correlation between coverage and robustness for deep neural networks (ASE 2019): [[Paper](https://arxiv.org/pdf/1911.05904.pdf)]



### **NMT Testing**

- [ ] Structure-Invariant Testing for Machine Translation (ICSE 2020): [[Paper](https://arxiv.org/pdf/1907.08710.pdf)]
- [ ] Automatic Testing and Improvement of Machine Translation (ICSE 2020): [[Paper](https://arxiv.org/pdf/1910.02688.pdf)]
- [ ] Metamorphic Testing for Machine Translations: MT4MT (ASWEC 2018):  [[Paper](https://arxiv.org/pdf/1910.02688.pdf)]

