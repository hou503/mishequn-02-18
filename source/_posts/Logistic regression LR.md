---
title: Logistic regression LR
date: 2020-12-09 18:20:30
tags: Algorithm
category: Algorithm
---

### Introduction

Logistic regression is a machine learning algorithm that is very popular in interviews, because on the surface it is formally very simple and easy to master, but when asked, it is easy to be confused. So the first advice to everyone in the interview is not to say they are proficient in logistic regression, it is very easy to be asked down, thus reducing the score. Here is a summary of some of the problems I usually encounter when interviewing others and being interviewed as an interviewer.

### Formal introduction

How to highlight that you are a person who already knows a lot about logistic regression. That is to summarize it in one sentence! Logistic regression assumes that the data obeys Bernoulli distribution, and uses gradient descent to solve the parameters by maximizing the likelihood function to achieve the purpose of dichotomizing the data.

This actually contains 5 points 

1: the assumptions of logistic regression, 

2: the loss function of logistic regression, 

3: the solution method of logistic regression, 

4: the purpose of logistic regression, 

5: how to classify logistic regression. 

These questions are to assess your basic understanding of logistic regression.

#### Basic assumptions of logistic regression
Any model has its own assumptions under which the model is applicable. The first basic assumption of logistic regression is that the data obeys the Bernoulli distribution. A simple example of the Bernoulli distribution is a coin toss, where the probability of a positive toss is 𝑝 and the probability of a negative toss is 1-𝑝. In the model of logistic regression, it is assumed that ℎ𝜃(𝑥) is the probability of a positive sample and 1-𝑝 probability and 1-ℎ𝜃(𝑥) is the probability that the sample is negative. Then the whole model can be described as

`ℎ𝜃(𝑥;𝜃)=𝑝`

The second hypothesis of logistic regression is to assume that the probability of the sample being positive is 

`𝑝=11+𝑒−𝜃𝑇𝑥`

So the final form of logistic regression 

`ℎ𝜃(𝑥;𝜃)=1/1+𝑒−𝜃𝑇𝑥`

#### Loss function of logistic regression
The loss function of logistic regression is its great likelihood function

`𝐿𝜃(𝑥)=∏𝑖=1𝑚ℎ𝜃(𝑥𝑖;𝜃)𝑦𝑖∗(1-ℎ𝜃(𝑥𝑖;𝜃))1-𝑦𝑖`

#### Solution method of logistic regression
Since this great likelihood function cannot be solved directly, we generally keep forcing the optimal solution by performing gradient descent on the function. There is actually a bonus point in this area to check your knowledge of other optimization methods. The interviewer may ask about the advantages and disadvantages of these three methods and how to choose the most suitable gradient descent method.

In simple terms, batch gradient descent will get the global optimal solution, the disadvantage is that when updating each parameter, you need to traverse all the data, the computation will be very large, and there will be a lot of redundant calculations, the result is that when the data volume is large, the update of each parameter will be very slow.

Stochastic gradient descent is updated frequently with high variance, which has the advantage of making sgd jump to new and potentially better local optimal solutions, and the disadvantage of making the process of convergence to local optimal solutions more complicated.

Small-batch gradient descent combines the advantages of sgd and batch gd by using n samples for each update. It reduces the number of parameter updates and can achieve more stable convergence results, and we generally use this method in deep learning.

In fact, there is a hidden deeper plus point here, depending on whether you understand optimization methods such as Adam, momentum method. Because the above methods actually have two fatal problems.
The first one is how to choose the right learning rate for the model. It is not appropriate to keep the same learning rate from the beginning to the end. At the beginning, when the parameters are just learning, the parameters are far away from the optimal solution, so it is necessary to keep a large learning rate to approach the optimal solution as soon as possible. However, when the parameters are learned later, the parameters and the optimal solution are already close to each other, and you still keep the initial learning rate, it is easy to cross the optimal point and oscillate back and forth near the optimal point, which, in layman's terms, makes it easy to overlearn and deviate.
The second is how to choose the appropriate learning rate for the parameters. In practice, it is not reasonable to keep the same learning rate for each parameter. Some parameters are updated frequently, so the learning rate can be smaller. Some parameters are updated slowly, so the learning rate should be larger. We will not expand here, and I will dedicate a special topic to it sometime.

#### Purpose of logistic regression
The purpose of this function is to dichotomize the data to improve the accuracy.

#### How logistic regression is classified
Logistic regression is a regression (i.e., the y-value is continuous), so how does it apply to classification. y-value is indeed a continuous variable. The practice of logistic regression is to delineate a threshold, and those with y-values greater than this threshold are in one category, and those with y-values less than this threshold are in another category. The threshold value is adjusted according to the actual situation. Generally, 0.5 will be chosen as the threshold for classification.

### Further questions about logistic regression

Although logistic regression is formally very simple, its connotation is very rich. There are many questions that can be considered

Why does the loss function of logistic regression use the maximum likelihood function as the loss function?
There are generally four types of loss functions, squared loss function, logarithmic loss function, HingeLoss0-1 loss function, and absolute value loss function. Taking the logarithm of the great likelihood function is equivalent to the logarithmic loss function. In this model of logistic regression, the training of the logarithmic loss function solves the parameters faster. As for the reason you can find out the gradient update of this equation

`𝜃𝑗=𝜃𝑗-(𝑦𝑖-ℎ𝜃(𝑥𝑖;𝜃))∗𝑥𝑖𝑗`

The update rate of this equation is only related to 𝑥𝑖𝑗, 𝑦𝑖. It is independent of the gradient of the sigmod function itself. This way the update speed is more stable from the beginning to the end.
Why not choose the squared loss function? One reason is that if you use the squared loss function, you will find that the gradient update speed is very correlated with the gradient of the sigmod function itself. sigmod function has a gradient no greater than 0.25 in its domain of definition. this will make the training very slow.

#### What is the impact of logistic regression if there are many features that are highly correlated or if a feature is repeated 100 times during the training process?
First of all, if the loss function eventually converges, even if there are a lot of highly correlated features, it will not affect the classifier's performance.

But for the features themselves, suppose there is only one feature, and you repeat it 100 times without considering sampling. After training, the data is still the same, but the feature itself is repeated 100 times, which essentially divides the original feature into 100 parts, each of which is one percent of the original feature weight value.

If in the case of random sampling, in fact, after training and convergence, we can still think that these 100 features and the original one feature play the same effect, but may be the value of many features in the middle of the positive and negative elimination.

#### Why do we still remove highly correlated features in the training process?

Removing highly correlated features will make the model more interpretable
It can greatly improve the speed of training. If there are many highly correlated features in the model, even if the loss function itself converges, the parameters actually do not converge, which will slow down the training speed. Secondly, more features will increase the training time.

### Summary of the advantages and disadvantages of logistic regression

When interviewing, people often ask what you feel when using logistic regression. What do you think are its advantages and disadvantages.

#### Here we summarize some advantages of logistic regression applied to industry.

The simplicity of the form and the interpretability of the model are very good. You can see the impact of different features on the final results from the weights of the features, and if the weight of a feature is high, then this feature will have a greater impact on the final results.

The model effect is good. It is acceptable in engineering (as baseline), if the feature engineering is done well, the effect will not be too bad, and the feature engineering can be developed by everyone in parallel, which greatly accelerates the speed of development.

The training speed is fast. When classifying, the amount of computation is only related to the number of features. And the distributed optimization of logistic regression sgd development is more mature, the training speed can be further improved by heaps of machines, so we can iterate several versions of the model in a short time.

The resource consumption is small, especially memory. Because only the feature values of each dimension need to be stored.

It is easy to adjust the output results. Logistic regression makes it easy to get the final classification results because the output is the probability score of each sample, and we can easily cutoff these probability scores, that is, divide the thresholds (those greater than a certain threshold are one class, those less than a certain threshold are one class).

#### But logistic regression itself has many drawbacks:

The accuracy rate is not very high. Because the form is very simple (very similar to a linear model), it is difficult to fit the true distribution of the data.

It is difficult to deal with data imbalance. For example, if we have a problem with very unbalanced positive and negative samples, such as a ratio of 10000:1, we can predict all the samples as positive and make the loss function smaller. But as a classifier, it will not be able to distinguish between positive and negative samples very well.

It is more troublesome to deal with nonlinear data. Logistic regression can only deal with linearly separable data without introducing other methods, or furthermore, with dichotomous problems .
Logistic regression itself cannot filter features. Sometimes, we use gbdt to filter features and then logistic regression on it.

