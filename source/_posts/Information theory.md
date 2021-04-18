---
title: Information theory
date: 2020-10-09 21:10:06
tags: Mathematics
category: Mathematics
---
Recent scientific research has consistently confirmed that uncertainty is the essential nature of the objective world. In other words, God really does roll the dice. An uncertain world can only be described using probabilistic models, and it was the depiction of probability that led to the birth of information theory.

In 1948, Claude Shannon, a physicist working at Bell Labs, published his famous paper "A Mathematical Theory of Communication", which provided a quantitative analysis of the qualitative concept of information and marked the birth of information theory as a discipline.

In A Mathematical Theory of Communication, Shannon begins, "The basic problem of communication is to reproduce at one point precisely or approximately the message selected at another point. Messages usually have meaning, i.e., according to some system, the message itself points to or relates to a physically or conceptually specific entity. But the semantic meaning of a message is irrelevant to the engineering problem; the important issue is that a message comes from a collection of all possible messages."

In this way, all types of messages are abstracted into logical symbols, which extends the scope of the communication task and the applicability of information theory, as well as completely divorcing the communication and processing of messages.

Information theory uses the concept of "information entropy" to explain the amount of information in a single source and the quantity and efficiency of information transmitted in communication, and to bridge the gap between the uncertainty of the world and the measurability of information.

Shannon's quantification of information was based on this idea, and he defined "entropy" as the most basic and important concept in information theory. The word "entropy" comes from another encyclopedic scientist, John von Neumann, who argued that no one knew what entropy was. Although the concept has been used extensively in thermodynamics, it was not until it was extended to information theory that the nature of entropy was explained, i.e., the degree of chaos inherent in a system.

In information theory, if an event A occurs with probability p(A), then the amount of self-information about this event is defined as

![](1.jpg)

The information entropy of a source containing multiple symbols can be calculated based on the amount of self-information of a single event. The information entropy of a source is the statistical average of the amount of self-information for each symbol that may be emitted by the source over the probability space constituted by the source. If a discrete source X contains n symbols and each symbol ai takes the value p(ai), then the source entropy of X is

![](2.jpg)

The source entropy describes the average amount of information provided by each symbol sent by the source and is the mean value of the overall information measure of the source. When each symbol in the source is taken with equal probability, the source entropy takes the maximum value log2n, meaning that the source is the most random.

There is the concept of conditional probability in probability theory, and extending conditional probability to information theory gives conditional entropy. If there is a correlation between two sources, the source entropy of the other source, Y, decreases if one of the sources, X, is known. The conditional entropy H(Y|X) represents the uncertainty of the other random variable Y given that the random variable X is known, i.e., the entropy calculated from the conditional probability of Y given X and then solved mathematically for X with the expectation that.

![](3.jpg)

The meaning of conditional entropy is that the variable Y is first classified once according to the value of the variable X, and its individual information entropy is computed for each divided category, and then the information entropy of each class is computed according to the distribution of X and its mathematical expectation.

In the case of a class, for example, where students can choose any seat in the classroom, there will be many possible seat distributions, and their source entropy will be larger. If a restriction is added to the choice of seats, such as boys sitting on the left and girls sitting on the right, the distribution of seats on the left and the distribution of seats on the right will still be random, but the situation will be much simpler compared to when no restriction is added. This is the reduction in uncertainty brought about by classification.

Once the conditional information entropy is defined, the concept of mutual information can be further obtained

![](4.jpg)

Mutual information is equal to the source entropy of Y minus the conditional entropy of Y when X is known, i.e., the removal of uncertainty about Y provided by X. It can also be seen as the information gain that X brings to Y. The information gain that X brings to Y can also be seen as the information gain. The name "mutual information" is often used in the field of communication, while "information gain" is often used in the field of machine learning, and the essence of both is the same.

In machine learning, information gain is often used in the selection of classification features. For a given training data set Y, H(Y) represents the uncertainty of classifying the training set when no features are given, while H(Y|X) represents the uncertainty of classifying the training set Y using feature X. H(Y|X) represents the uncertainty of classifying the training set Y using feature X when no features are given. The gain of information represents the degree to which the uncertainty of classification of the training set Y is reduced by feature X, i.e., how well feature X distinguishes the training set Y from other features.

Obviously, features with greater gain of information have better classification ability. However, the value of information gain depends heavily on the information entropy H(Y) of the dataset, and thus does not have absolute significance. To solve this problem, researchers have also developed the concept of information gain ratio and defined it as g(X,Y) = I(X;Y)/H(Y).

Another information theoretic concept often used in machine learning is called "Kullback-Leibler scatter", or KL scatter, which is a method to describe the difference between two probability distributions P and Q. KL scatter is defined as follows

![](5.jpg)

KL scatter is a measure of the amount of additional information. Given a source with a probability distribution of symbols P(X), one can devise an optimal encoding for P(X) such that the least number of average bits (equal to the source entropy of the source) is required to represent the source.

However, when the set of symbols of the source remains unchanged, and the probability distribution of conformity changes to Q(X), then the optimal encoding for P(X) is used to encode the symbols of the distribution of conformity Q(X), and the number of characters in the resultant encoding will be a few bits more than the optimal value.

The KL scatter is a measure of the average number of extra bits per character in this case, and can also represent the distance between the two distributions.

Two important properties of KL scatter are non-negativity and asymmetry.

Non-negativity means that the KL sparsity is greater than or equal to 0, and the equal sign is only taken when the two distributions are identical.

Asymmetry means that DKL(P||Q) ≠ DKL(Q||P), i.e., the deviation obtained by approximating Q(X) with P(X) is different from that obtained by approximating P(X) with Q(X), and thus the KL dispersion does not satisfy the mathematical definition of distance, which is important to note.

In fact, DKL(P||Q) and DKL(Q||P) represent two different ways of approximating the distance. For DKL(P||Q) to be minimal, it is necessary to have Q(X) equally not equal to 0 at locations where P(X) is not equal to 0. For DKL(Q||P) to be minimal, it is necessary to have Q(X) equally equal to 0 at locations where P(X) is equal to 0.

In addition to the indicators defined above, there is another important theorem in information theory, called the "maximum entropy principle". The principle of maximum entropy is a criterion for determining the statistical properties of random variables in an attempt to best fit the objective situation. The worst-case scenario for an unknown probability distribution is that it takes every possible value with equal probability. This is the time when the probability distribution is most uniform, which means that the random variable is the most random, and predicting it is the most difficult.

From this point of view, the essence of the principle of maximum entropy is that it does not introduce any unnecessary constraints or assumptions when extrapolating an unknown distribution, and therefore the most uncertain results can be obtained with the least risk in prediction. The famous saying "Don't put all your eggs in the same basket" in investment finance can be regarded as a practical application of the principle of maximum entropy.

The maximum entropy model can be obtained by applying the principle of maximum entropy to a classification problem. In the classification problem, first of all, it is necessary to determine some characteristic functions as the basis of classification. In order to ensure the validity of the feature functions, their mathematical expectations on the true distribution P(X) of the model and on the empirical distribution P~(X) derived from the training dataset should be equal, i.e., the estimate of the mathematical expectations of a given feature function should be an unbiased estimate.

In this way, each feature function corresponds to a constraint. The task of classification is to determine the best classification model given these constraints. Since there is no a priori knowledge of classification other than these constraints, it is necessary to solve the distribution of conditions with the greatest uncertainty using the principle of maximum entropy, i.e., let the following functions maximize their values

![](6.jpg)

where p(y|x) is the distribution of objective conditions to be determined for the classification problem. Calculating the maximum value of the above equation is essentially a constrained optimization problem, and the constraints determined by the eigenfunction can be transformed into an unconstrained optimization problem by removing its influence through the introduction of the Lagrange multiplier. It can be proven mathematically that the solution to this model exists and is unique.

Today I share with you the essential information-theoretic foundations of AI, focusing on the interpretation of abstract concepts rather than the derivation of mathematical formulas, the main points of which are as follows.

- Information theory deals with uncertainty in the objective world.
- Conditional entropy and information gain are important parameters in classification problems.
- KL scatter is used to describe the difference between two different probability distributions.
- The principle of maximum entropy is a common criterion in classification problems.

