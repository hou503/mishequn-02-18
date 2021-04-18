---
title: Plain Bayesian
date: 2020-11-29 14:01:28
tags: Algorithm
category: Algorithm
---


### Idea.

For a given item x to be classified, the posterior probability distribution is calculated by the learned model, i.e., the probability of occurrence of each target class under the condition that this item occurs, and the class with the highest posterior probability is taken as the class to which x belongs. The posterior probability is calculated according to Bayes' theorem.

Key: To avoid the combinatorial explosion and sample sparsity problems faced in solving Bayes' theorem, the assumption of conditional independence is introduced. The features used for classification are conditionally independent under the condition that the classes are determined.

### What is plain Bayesian plain?

Simply put: When solving the joint probability P(XY) using Bayes' theorem, the conditional probability P(X|Y) needs to be computed. In calculating P(X|Y), the plain Bayesian makes a strong assumption of conditional independence (when Y is determined, the components of X take values independent of each other), i.e., P(X1=x1,X2=x2,... ...Xj=xj|Y=yk) = P(X1=x1|Y=yk)*P(X2=x2|Y=yk)*... *P(Xj=xj|Y=yk).

Main applications: email spam filtering, news classification

Before training the plain Bayesian classifier, the training set is processed and the text is cleaned or there is much to learn.

The text is vectorized according to the extracted classification features, and then the plain Bayesian classifier is trained.

The difference in the number of de-high frequency words has an impact on the results.

Laplace smoothing has a positive effect on improving the classification of the plain Bayes classifier.


### What is the difference between plain Bayesian and LR?

In brief.

(1) Parsimonious Bayes is a generative model, which learns the prior probability P(Y) and conditional probability P(X|Y) based on Bayesian estimation of existing samples, and then finds the joint distribution probability P(XY), and finally solves P(Y|X) using Bayes' theorem, while LR is a discriminant model, which directly finds the conditional probability P(Y|X) based on the maximized log-likelihood function.

(2) Parsimonious Bayes is based on a strong assumption of conditional independence (each feature variable takes values independently of each other under the condition that the classification Y is known), whereas LR does not require this.

(3) Parsimonious Bayes is applicable to scenarios with small data sets, while LR is applicable to large-scale data sets.

The former is a generative model and the latter is a discriminative model.

1) First, Navie Bayes finds the prior probability P(Y), and the conditional probability P(X|Y) from known samples, and for a given instance, calculates the joint probability, and then finds the posterior probability. That is, it tries to find exactly how this data was generated (produced) and then classify it. Whichever category is most likely to generate this signal belongs to that category.

Advantages: faster convergence when the sample size increases; applicable when hidden variables are present.

Disadvantages: long time; requires more samples; wastes computational resources

2) In contrast, logistic regression does not care about the proportion of categories in the sample and the probability of features appearing under the categories; it gives the equation of the prediction model directly. Let each feature have a weight, and the training sample data update the weight w to derive the final expression. Gradient method.

Advantages: direct prediction tends to be more accurate; simplifies the problem; can reflect the distribution of the data, the differential characteristics of the categories; applicable to the identification of more categories.

Disadvantages: slow convergence; not applicable to cases with hidden variables.

### What if the probability is 0 when estimating the conditional probability P(X|Y)?

In short: introduce λ. When λ=1, it is called Laplace smoothing. 

### Advantages and disadvantages of plain Bayesian

Advantages: good performance for small-scale data, suitable for multi-classification tasks, and suitable for incremental training.

Disadvantages: sensitive to the expression form of the input data (discrete, continuous, extremely small values, etc.).

### Why is the assumption of attribute independence difficult to hold in practical situations, but plain Bayes still achieves better results?

1) For classification tasks, as long as the conditional probabilities of each category are correctly ordered and no precise probability values are needed to lead to correct classification.

2) If inter-attribute dependencies affect all categories equally, or if the effects of dependencies can cancel each other out, the assumption of conditional independence of attributes does not negatively affect performance while reducing computational overhead.

### Why a posteriori probability maximization.

Equivalently, expect risk minimization. Suppose a 0-1 loss function is chosen, i.e., 1 for correct classification and 0 for error, when the expected risk is minimized as

![1393464-20180504114329112-1062579832.png](1393464-20180504114329112-1062579832.png)

### Algorithmic problems.

In actual projects, the probability values are often very small decimals, and multiplying successive tiny decimals can easily cause the underflow to make the product 0.

Solution: Take the natural logarithm of the product, and change the multiplication into a continuous addition.

Also need to note: the length of the given feature vector may be different, which needs to be normalized to the through-length vector (here to text classification for example), for example, if it is a sentence word, the length is the length of the entire vocabulary, the corresponding position is the number of times the word appears.

### Calculation of the prior conditional probability.

- When discrete distribution: statistics of the frequency of occurrence of each category in the training sample. If the probability of a certain eigenvalue is 0 will make the whole probability product become 0 (called data sparsity), which destroys the assumption that each eigenvalue has the same status.

Solution 1: Using Bayesian estimation (called Laplace smoothing when λ = 1).

![1393464-20180504114346515-581861539.png](1393464-20180504114346515-581861539.png)

Solution 2: By clustering the non-occurring words to find out the system keywords, and find the average value according to the probability of related words.

- When continuous distribution: assume that its value obeys Gaussian distribution (normal distribution). That is, the sample mean and variance are calculated.

