+++
date = "26 Jan 2018"
draft = true
title = "Class 1: Intro to Adversarial Machine Learning"
author = "Team Panda"
slug = "class1"
+++

_TODO: list complete references (at least full author lists,
publication venue, year) for the papers covered, and links to the
paper_

## Machine Learning Background
<!-- - Other ML algorithms use linear decision boundaries (SVM, LR, ...)
- Deep learning uses linear units with nonlinear composition
	- More susceptible to attacks
- Define terms like gradient and loss -->

In supervised Machine Learning, we train models with training data
along with the label associated with it. We extract features from each
sample, and use an algorithm to train a model where the inputs are
those features and the output is the label. 

For classifying the testing data, the classifier uses decision
boundary to separate points of the data belonging to each class. In a
statistical-classification problem with two classes, a decision
boundary partitions all the underlying vector space into two separate
classes. A support vector machine (SVM) is a linear classifier which
constructs a boundary by focusing two closest points from different
classes and it finds the line that is equidistant to these two points.

#### Loss Functions

A loss function define that how good a given model is at making
predictions for a given scenario. It has its own curve and its own
gradients. The slope of the curve indicates the appropriate way of
updating the parameters to make the model more accurate in case of
prediction. A frequently used loss function is the 0-1 loss function.

<p align="center">
<img src="/images/01_loss_function.png" width="150" >
</p>


where \\(I\\) is the indicator notation. Hinge loss function is also popular in machine learning field. It provides a relatively tight, convex upper bound on the 0-1 indicator function.

<!-- <p align="center">
<img src="/images/hinge_function.png" width="150" >
</p> -->

#### Gradient Descent
Gradient Descent is an optimization algorithm which is used to minimize cost function by iteratively moving the direction of the steepest descent. In machine learning, we use gradient descent to update the parameters (coefficients in Linear Regression and weight in neural networks) of our model. 

   <p align="center">
<img src="/images/gradient_descent_graph.png" width="300" >
<br>
_TODO: add image credit_
   </p>

In the figure, weight is slowly decreasing from the initial stage. \\(J_{min}\\) is the minimum value of the cost function which can be obtained by optimizing the algorithm. 

Recently, there have been many successful applications of Deep Neural
Networks (DNN) in the fields of information retrieval, computer
vision, and speech recognition. Despite their potential, deep neural
architectures, like all other machine learning approaches, are
vulnerable to what are known as adversarial samples. These systems can
be fooled by targeted manipulations which slightly modifying a real
example to trick the model into “believing” that this modified sample
belongs to an incorrect class with high confidence. To defend against
these adversarial attacks, many researchers are exploring several
directions including data augmentation, increasing model complexity,
retraining, pre-processing, etc. Their main goal is to protect the
model against adversarial manipulations by the attackers.

---
## Applications and Vulnerabilities
	
To get a better idea of what security and privacy mean in a machine learning context, we highlight a few
applications and examine the possible consequences of vulnerabilities in these domains.

One of the most commonly-discussed applications is in self-driving
cars, which use machine learning algorithms for image recognition
tasks, such as identifying traffic signs and road hazards.  If an
attacker were to cause an erroneous classification, e.g. mistaking a
stop sign for a speed limit sign or missing a pedestrian in a
crosswalk, both property and human lives would be in
jeopardy. (Although luckily self-driving cars use a variety of other
techniques to avoid collisions, including detailed maps and LIDAR, so
are likely to not be directly vulnerable to image mis-classification
attacks.)

Machine learning has also found a place in the medical world, being used to identify patterns and trends in patient histories that can be used for diagnoses, to recognize abnormalities in medical imaging, and even to evaluate the efficiency of a hospital's workflow.  In healthcare, the risks are two-fold - there are both safety and privacy risks that need to be considered.  An attacker could manipulate the models used for diagnostics and pattern recognition, thereby putting patient health at risk, and there is also the potential for confidential patient data to be leaked either through the training process or the final model itself. _TODO: I don't see the adversarial scenario here. There is an important issue with insurance fraud, but I have a hard time imagining a scenario where an adversary would want to manipulate a patient's scan images._

There are numerous other uses of ML today (targeted advertising, fraud detection, malware detection, etc.) and each carries particular security and privacy concerns.

---
## Adversarial Examples

In machine learning, an adversarial example is an input that has been manipulated so that the model returns a different, incorrect output.  _TODO: provide some background on this, especially because of the history from the Biggio paper_
A classic adversarial example for image classification is from a paper by Goodfellow et al. _TODO: add cite and link_, which features a photo of a panda that was originally classified correctly, but is misclassified when carefully-crafted noise is added.  The model classifies the new image as a gibbon, even though human eyes can easily see it is a panda. 

   <p align="center">
<img src="/images/panda.png" width="600" >
<br>_TODO: image credit_
   </p>

### Classifying Attacks

Attacks can be categorized by many different characteristics, but are often referred to in terms of the attack method, the goal of the attack, and the adversary's level of knowledge.
_TODO: if this is following the taxonomy from the Papernot et al. paper, mention and link_

#### Attack Method

A poisoning attack is when the adversary adds carefully-crafted samples into the training data, thereby disrupting the learning process and manipulating the final trained model.  An evasion attack occurs when a sample originally correctly classified into class A is manipulated and fed back into the model, now receiving an output that is *not* class A.  A key difference between these two attacks is where each occurs in the ML pipeline: poisoning attacks occur before the training stage and evasion attacks occuring after training, during the testing stage.

   <p align="center">
<img src="/images/ml_pipeline.jpg" width="650" >
<br>_TODO: image credit_
   </p>

#### Goal of Attack

The goal of the adversary is another way to categorize attacks, which
in this case means whether or not there was a specific goal
classification or not.  _Error-generic_ (also called _untargeted_)
attacks aim to find a sample close to a given seed that is
misclassified, but do not have a specific target output
class. _Error-specific_ (_targeted_) attacks deliberately change the
seed sample's classification from the original class A to a chosen
class B.

#### Adversary Knowledge

The level of knowledge the adversary possesses generally falls into one of three loosely defined categories: black box, grey box, and white box attacks.  Black box attacks assume the lowest level of adversary knowledge, generally only allowing the final classification to be known without any finer details about the model.  On the other end of the spectrum, white box attacks assume that the attacker knows everything about the model, including the specific algorithm used, the feature set, the training data, etc. An attack by an adversary who knows more about the model than a black box but less than a white box is called a grey box attack, although the exact definition tends to vary.

<p align="center">
<img src="/images/boxes.png" width="600" >
</p>


---
## Modern Machine Learning as a Potemkin village

<p align="center">
<img src="/images/potemkin.jpeg" width="350" >
</p>

The term "Potemkin village" describes a difference between appeance and reality. In a [paper](https://arxiv.org/abs/1412.6572) by Goodfellow, Shlens, and Szegdy, modern machine learning methods are described as building "a Potemkin village that works well on naturally occuring data, but is exposed as a fake when one visits points in space that do not have high probability in the data distribution." The apparent fragility of deep neural networks has been particuarly exposed, as seen in the panda picture above. 

The two-faced nature of machine learning models to be both fragile and robout from two different angles leads to concerns in the security of machine learning algorithms. High confidence in classifying adversarial examples poses a threat to the integrity of the model's output.

#### Fast gradient sign method (FGSM) 

Also the [paper](https://arxiv.org/abs/1412.6572) mentioned a method for generating adversarial examples. Fast gradient sign method maximizes the error between the ground truth classification of the sample and minimizes the error to the target classification. The method is effecitve is generating the best pixels to change to achieve a higher loss, essentially acting as a reverse optimization method. FGSM simply traverses the loss curve by moving in the opposite direction of the gradient loss. 

<p align="center">
<img src="/images/formula.png" width="200" >
</p>

As we discussed, the sign method in the FGSM is an approximation to the actual gradient. Although the most accurate method would use the true gradient, the sign of the gradient is taken for the sake efficiency: since \\(\epsilon\\) reflect the adversarial strength (the maximum size perturbation allowed), this represents taking the largest step possible in each dimension in the direction given by the gradient. (There are many variations on FGSM that we'll discuss in later posts, including iterative versions where a sequence of smaller steps is used.) FGSM showcases the fragility of machine learning models especially through visualizations such as the panda example. 

---
## Adversarial training
#### Effects of nonlinearities
Nonlinearities lead to vulnerabilities. As shown below, there are spaces around the mode of the function, which can exploited by adversarial examples.

   <p align="center">
<img src="/images/capacity.png" width="650" >
<br>_TODO: add image credit_
   </p>

#### Solutions
- Train model on mixture of original and adversarial examples - Szegedy et al.
- Use an adversarial regularizer - Goodfellow et al.

_TODO: looks like this wansn't completed - need to include references and links, and explain what each of these is (and how well they actually work)_

---
## Transferability of adversarial samples
As shown above, an adversarial sample can fool a machine-learned model to misclassify it with high confidence. In particular, the adversarial samples made to fool image classifiers show how an adversarial sample can be indistinguishable to humans from legitimate samples. Additionally, examples that evade one classifier tend to evade others. This poses a vulnerability for machine learned models, one that can be exploited with cross-evasion.

#### Learning classifier substitutes
For example, in this [paper](https://arxiv.org/abs/1605.07277), Papernot et al. took advantage of the transferability of adversarial examples on a remote DNN by training a new model with their own synthetic data. By using a relatively small number of queries to label their data, they trained a substitute model which they then used to craft adversarial samples. Demonstrating transferability, 84.24% of the adversarial samples trained from the substitute model fooled the remote DNN.

_TODO: a bit more explanation of this result - how the experiments were done, how transferability differs based on the different models, etc._

--- Team Panda