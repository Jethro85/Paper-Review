# Design and Evaluation of a Multi-Domain Trojan Detection Method on Deep Neural Networks 

Label: Multi-Domain, Backdoor Detection

---

## SUMMARY

Trojan attacks on deep neural networks (DNNs) exploit a backdoor embedded in a DNN model to hijack any input with an attacker’s chosen signature trigger. All emerging defence mechanisms are only validated on vision domain tasks on 2D Convolutional Neural Network (CNN) model architectures; **whether a defence mechanism is general across vision, text, and audio domain tasks remains unclear**. This work corroborates a run-time Trojan detection method exploiting STRong Intentional Perturbation of inputs, is a multi-domain Trojan detection defence across Vision, Text and Audio domains—thus termed as STRIP-ViTA. Speciﬁcally, STRIP-ViTA is the ﬁrst conﬁrmed Trojan detection method that is demonstratively independent of both the task domain and model architectures. **(Briefly, this paper apply their former Trojan detection method 'STRIP' to some other different domains besides vision domain, like text domain and audio domain)**

## MAIN IDEA 

From my perspective, the main idea they used in this paper is just like the one in their former paper: 'STRIP: A Defence Against Trojan Attacks on Deep Neural Networks'.   

![1.JPG-117.8kB][1]

The perturbation step generates N perturbed inputs {xp1,......,xpN} corresponding to one given incoming input x.

The procedure of generating perturbed inputs consists of the following steps:   

1) Produce N copies (replica) of input x;   
2) Produce N perturbation patterns pi with i ∈{1,...,N};   
3) Use the produced N perturbation patterns to perturb each input replica to gain perturbed input xpi.   

Then, all perturbed inputs along with x itself are concurrently fed into the deployed DNN model, FΘ(xi). According to the input x, the DNN model predicts its label z. At the same time, the DNN model determines whether the input x is Trojaned or not based on the observation on predicted classes to all N perturbed inputs {xp1,......,xpN} that forms a set Dp—the randomness of the predicted classes can speciﬁcally be used for testing a given input is Trojaned or not.

## EXPERIMENTS

To evaluate the performance of STRIP-ViTA, they implemented Trojan attacks on DNNs for each of vision, text, and audio domain. Then they applied STRIP-ViTA with developed perturbation methods to detect Trojan inputs on those models, respectively.

Attack success rate and classification accuracy of trojan attacks on tested tasks are shown below. 

![1.JPG-135kB][2]

The main differences between their experiments lay on the perturbation methods they use. 

- For vision task, the input is image. We adopted the image superimpose as the perturbation method. Speciﬁcally, each perturbed input xpi is a superimposed image of both the input x (replica) and an image randomlydrawnfromtheuserheld-outdataset.  
-  In contrast to the perturbation method, superimpose, for vision task. Adding words together is not suitable in the text domain. Here, they use word replacement. When the input x comes, they randomly replace a fraction of words, e.g., m words, in the replicated input x. Speciﬁcally, to perturb each replicated input x, they follow below steps: (1) randomly drawing a text sample from the held-out dataset; (2) ranking words in the text sample with frequency-inverse document frequency (TFIDF) score of each word in the sample text. TFIDF represents how important each word is in the sample text; (3) picking up m words with highest TFIDF score and using them to replace the m words randomly chosen in the replicated input x. 
-  Perturbing audio input is similar to perturb image input. They simply add the input replica with the randomly drawn perturbation sample from the held-out dataset.

They did lots of experiment to test the performance of STRIP-ViTA, including testing the performance of Detection Capability, Insensitivity to Model Complexity, Trigger Size Independence, Multiple Infected Labels with Separate Triggers Scenario and Same Infected Label with Separate Triggers Scenario. 

I will not show the results of all of the experiments. But from their results, I can conclude that STRIP-ViTA is efﬁciently against all the shown advanced Trojan attacks. The crucial reason is that STRIP-ViTA exploits the input-agnostic Trojan characteristic—the main strength of such attack, while it is independent on trigger shape, size or/and other settings. In other words, as long as the trigger is input-agnostic and a high attack success rate is desired—always the case for the attacker, STRIP-ViTA will be effective regardless Trojan attack variants.

## COMMENTS
It's true that all emerging defence mechanisms are only validated on vision domain tasks on 2D Convolutional Neural Network (CNN) model architectures. So whether a defence mechanism is general across vision, text, and audio domain tasks? This is a new point of view to test a Trojan detection or mitigation method. 



  [1]: http://static.zybuluo.com/Shenao/76pgwksc6415p2q4hsyrb8uo/1.JPG
  [2]: http://static.zybuluo.com/Shenao/juemcyq8paob1iky1zjk3oi2/1.JPG