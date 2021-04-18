---
title: Integrated Learning
date: 2020-12-20 21:22:56
tags: Algorithm
category: Algorithm
---

## The concepts of boosting and bagging. 

(1) bagging: random sampling from the original data to get S datasets of the same size to train S base learners, which are not dependent on each other. It is a parallel method. 

The weights of each classifier are equal. The classification results are classified using these S classifiers, and the category with the most classifier voting results is selected as the final classification result.(The sampling method is sampling with put-back: allowing that there can be duplicate values in each small data set.)

There is no restriction on bagging for weak learners, as with Adaboost. But the most common ones are generally decision trees and neural networks.

The aggregation strategy of bagging is also relatively simple. For classification problems, simple voting is usually used, and the category or one of the categories with the most votes is the final model output. For regression problems, simple averaging is usually used, where the final model output is obtained by arithmetic averaging the regression results obtained by T weak learners.

Advantages. 


a. The algorithm samples each time to train the model, which has a strong generalization capability and is useful for reducing the variance of the model, but of course the fit to the training set will be worse, i.e., the model bias will be larger.

b. Training a Bagging integration is of the same order of complexity as training a learner directly using the base learning algorithm, which is efficient.

c. Standard AdaBoost is only applicable to binary classification, Bagging can be directly used for tasks such as multi-classification, regression, etc..

d. Because of self-sampling, each base learner uses only about 63.2% of the samples in the training set of the accident, and the remaining samples (36.8% called OOB) can be used as the validation set, etc.

(2) boosting: using all the data to train the base learner, there is a dependency between individual learners, each learner is based on the results of the previously trained learners, serial training, focus on the data that was wrongly scored to get a new learner, to achieve the effect of improvement. (In layman's terms, this means learning one point at a time and then approaching the final value to be predicted step by step.) 

The classification result is based on the weighted sum of all classifiers, and the weights of the classifiers are not equal, each weight represents the success of its corresponding classifier in the previous iteration.

Advantages: low generalization error, easy to implement, high classification accuracy, few adjustable parameters.

Disadvantages: more sensitive to outlier points.

Both are identical: the types of classifiers used are the same.

## Why is bagging to reduce variance variance, while boosting to reduce bias bias?

　　(1) Bagging resamples the samples, trains a model for each resampled subset, and finally takes the average. Due to the similarity of the subsample sets and the use of the same model, each model has approximately equal bias and variance (in fact, the distribution of each model is also approximately the same, but not independent). bagging method to obtain each submodel is somewhat correlated, belongs to the intermediate state of the above two extreme conditions, so it can reduce the variance to some extent. variance, prediction is more focused)

　　(2) Therefore, boosting is minimizing the loss function sequentially (in series), and its bias naturally decreases gradually. (2) Thus, boosting is minimizing the loss function sequentially, and the bias decreases gradually. (lower bias, more accurate prediction)

　　(3) Intuitive explanation 
　　boosting is the combination of many weak classifiers into one strong classifier. Weak classifiers have high bias, while strong classifiers have low bias, so boosting plays a role in reducing bias. variance is not the main consideration for boosting. 
　　Bagging is averaging over many strong (or even overly strong) classifiers. Here, the bias of each individual classifier is low, and the bias remains low after averaging; and each individual classifier is strong enough to produce overfitting, i.e., the variance is high, and the averaging operation serves to reduce this variance.

## The main differences between the two

Sample selection: Bagging uses Bootstrap random with put-back sampling; while Boosting's training set is constant for each round, and only the weight of each sample is changed.

Sample weights: Bagging uses uniform sampling, and each sample is equally weighted; Boosting adjusts the sample weights according to the error rate, and the larger the error rate, the larger the sample weight.

Prediction function: All the prediction functions of Bagging have equal weights; the smaller the error in Boosting, the greater the weight of the prediction function.

Parallel computation: Each prediction function of Bagging can be generated in parallel; each prediction function of Boosting must be generated sequentially and iteratively.

## The following are the new algorithms obtained by combining decision trees with these algorithmic frameworks.

1) Bagging + Decision Tree (CART) = Random Forest

2) AdaBoost + Decision Tree = Boosted Tree

3) Gradient Boosting + Decision Tree = GBDT

## Possible benefits of combining learners 　　

(1) Improved generalization 

(2) Reduced risk of local optimum 

(3) Expanded hypothesis space and better similarity.


## Methods/strategies for model fusion 

(1) Averaging: For numerical regression prediction problems, the commonly used combination strategy is averaging, that is, averaging the outputs of several weak learners to obtain the final prediction output.

(2) Voting method: The simplest voting method is the relative majority voting method, which is often referred to as minority rule.

(3) Learning method: stacking (the output of this layer is used as part of the input data of the next layer)

When using the combination strategy of stacking, instead of doing simple logic processing on the results of the weak learner, we add another layer of learners, that is, we take the learning results of the weak learner in the training set as input and the output of the training set as output, and retrain a learner to get the final results.

##  Principles of common fusion frameworks; advantages and disadvantages; will fusion necessarily improve performance? Why fusion may improve the prediction effect? 

Principle: more than one is better than one + ensure accuracy, prevent overfitting + weak learners obviously + good but different 

Common: bagging (parallel + less variance), boosting (serial + less bias), stacking (output->input) 

Not necessarily, good but different 

Models differ, reflect different expressive power
