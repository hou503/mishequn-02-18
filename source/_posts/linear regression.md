---
title: linear regression
date: 2020-11-11 22:45:09
tags: AI
category: AI
---

In mathematical statistics, regression analysis is a method of determining quantitative relationships among multiple variables that are interdependent. Linear regression assumes that the output variable is a linear combination of several input variables and solves for the optimal coefficient in the linear combination based on this relationship. Among the many methods of regression analysis, the linear regression model is the easiest to fit and the statistical properties of its estimated results are easier to determine, thus it is widely used. In machine learning, the regression problem implies the premise that both the input and output variables can be taken continuously, and thus the linear regression model can be used to give estimates of the output for any input.

In 1875, Francis Galton, a British statistician working on genetic problems, was looking for the relationship between parental and offspring height. After analyzing the height data of 1078 father-son pairs, he found that the scatter plot of these data was roughly linear, that is, the height of the father was positively correlated with the height of the son. Behind the positive correlation lies another phenomenon: sons of short fathers are more likely to be taller than their fathers, while sons of tall fathers are more likely to be shorter than their fathers.

Influenced by his cousin Charles Darwin, Galton called this phenomenon the "regression effect," which means that nature constrains the distribution of human height to a relatively stable and non-polarizing overall level, and gave the first expression for linear regression in history: y = 0.516x + 33.73, in which y and x represent the height in inches of the offspring and the parent, respectively.

Galton's ideas are still alive and well in today's machine learning. Assuming that an instance can be represented by a column vector x=(x1;x2;⋯,xn), with each xi representing the instance's value on the i-th attribute, linear regression works by acquiring a set of parameters wi,i=0,1,⋯,n so that the predicted output can be represented as a linear combination of the instance's attributes with this set of parameters as weights. If the constant x0=1 is introduced, the model that linear regression attempts to learn is

![](1.jpg)

When the instance has only one attribute, the relationship between input and output is a straight line on a two-dimensional plane; when the number of attributes of the instance is large, linear regression yields a hyperplane on an n-dimensional space, corresponding to a linear subspace of dimension equal to n - 1.

When determining the coefficient wi on the training set, the error between the predicted output f(x) and the true output y is the central metric of interest. In linear regression, this error is defined in terms of the mean squared error. When the linear regression model is a straight line on a two-dimensional plane, the mean square error is the Euclidean distance between the predicted output and the true output, which is the L2 parametrization of the vector between two points. The solution method of the model with the goal of minimizing the mean square error is the least squares method, which can be written as follows

![](2.jpg)

where each xk represents a sample in the training set. In the univariate linear regression task, the role of least squares is to find a straight line that minimizes the sum of the European distances from all samples to the line.

At this point, the question arises: why should the parameter that minimizes the mean square error be the optimal model that matches the training samples?

This problem can be elucidated from a probabilistic point of view. Linear regression yields a statistically significant fit, and in the univariate case, it is possible that every sample point does not fall on the resulting line.

One explanation for this phenomenon is that the regression results can perfectly match the distribution of the ideal sample points, but the real sample points used in the training are the result of the superposition of the ideal sample points and the noise, thus creating a deviation from the regression model, and the value of the noise at each sample point is equal to yk - f(xk).

It is assumed that the noise affecting the sample points satisfies a normal distribution with parameter (0,σ2) (remember the probability density formula for a normal distribution?) This means that the probability density is greatest for noise equal to zero, and the larger the magnitude (whether positive or negative), the smaller the probability of noise occurring. In this case, the derivation of the parameter w can be done in a maximum likelihood manner, i.e., finding the assumptions that make the sample data appear with maximum probability, given that the sample data and its distribution are known.

The probability of a single sample xk is actually the probability that the noise is equal to yk - f(xk), while the probability of the simultaneous occurrence of all samples independent of each other is the product of the probabilities of each sample, which can be written as follows

![](3.jpg)

The task of maximum likelihood estimation is to maximize the values of the above expressions. For computational simplicity, the product of the above expression can be transformed into a summation by taking the logarithm, and the logarithm operation does not affect its monotonicity. After some calculations, the maximization of the above formula is equivalent to the minimization of ∑k(yk-wTxk)2. Isn't this the result of least squares?

Thus, for univariate linear regression, where the error function obeys a normal distribution, least squares from a geometric sense is equivalent to maximum likelihood estimation from a probabilistic sense.

Having determined the optimality of the least squares method, the next question is how to solve for the minimum value of the mean squared error. In univariate linear regression, the regression equation can be written as y=w1x+w0. According to the optimization theory, substitute this expression into the mean square error expression, and find the derivatives of w1 and w0 respectively, so that the value of both derivatives is equal to 0 is the optimal solution of the linear regression, and its analytical formula can be written as follows

![](4.jpg)

Univariate linear regression is just the simplest of exceptions. The height of the offspring is not determined solely by the genetic inheritance of the parents; factors such as nutritional conditions and living environment can have an effect. When the description of the sample involves multiple attributes, this type of problem is called multiple linear regression.

The parameters w in multiple linear regression can also be estimated using least squares, and their optimal solutions are also determined using partial derivatives, but the elements involved in the operation change from vectors to matrices. In the ideal case, the optimal parameters of multiple linear regression are

![](5.jpg)

where X is a matrix of transpositions of all samples x=(x0;x1;x2;⋯,xn) together. However, this expression holds only in the presence of the inverse of the matrix (XTX). In a large number of complex practical tasks, the number of attributes in each sample may even exceed the total number of samples in the training set, where the optimal solution w∗ will not be unique and the choice of solution will depend on the inductive preferences of the learning algorithm.

However, regardless of the selection criteria used, the fact that there are multiple optimal solutions cannot be changed, which implies the generation of overfitting. More importantly, in the case of overfitting, a small perturbation bringing a millisecond difference to the training data may cause the trained model to be false and the stability of the model cannot be guaranteed.

A common approach to solving the overfitting problem is regularization, i.e., adding an additional penalty term. In linear regression, there are two types of regularization methods according to the penalties used, namely "Ridge Regression" and "LASSO Regression".

In machine learning, Ridge Regression, also known as Parameter Decay, was proposed by Andrei Tikhonov in the 1940's. Of course, machine learning was not yet in existence at that time. The main purpose of this method was to solve the stability problem of matrix inverse, and its idea was later applied to regularization, which resulted in today's Ridge Regression.

Ridge regression implements regularization by adding a two-paradigm term to the original mean square error term to be solved, i.e., the minimized object becomes ||yk-wTxk||2+||Γw||2, where Γ is called a Tikhonov matrix and can be reduced to a constant.

From an optimization point of view, the role of the two-paradigm penalty term is to preferentially select w with a smaller paradigm, which is equivalent to adding an extra layer of constraints on the properties of the optimal solution in addition to the minimum mean square error, restricting the optimal solution to a ball in a high-dimensional space. The Ridge regression is equivalent to scaling the original least-squares result, and although the contribution of each parameter in the optimal solution is weakened, the number of parameters does not become smaller.

The full name of LASSO regression is "Least Absolute Shrinkage and Selection Operator" (LASSO), which was proposed by Robert Tibshlani in 1996. In contrast to the Ridge regression, the LASSO regression selects a paradigm term of the parameter to be solved as the penalty term, i.e., the minimization object becomes ||yk-wTxk||2+λ||w|||1, where λ is a constant.

In contrast to the Ridge regression, the LASSO regression is characterized by the introduction of sparsity. It reduces the dimensionality of the optimal solution w, i.e., it weakens the contribution of some of the parameters to zero, which makes the number of elements in w much smaller than the number of original features.

This can be seen more or less as an implementation of Occam's razor principle: when both primary and secondary contradictions exist, the priority must be the primary contradiction. While factors such as diet, environment, and exercise all influence variation in height, the determining factor is clearly present only in the chromosomes. It is worth mentioning that the introduction of sparsity is a common method for simplifying complex problems and is widely used in other fields such as data compression and signal processing.

From a probabilistic point of view, the analytical solution of least squares can be obtained using normal distribution as well as maximum likelihood estimation, as explained previously. Ridge regression and LASSO regression can also be interpreted from the probabilistic point of view: Ridge regression is estimated with the maximum a posteriori probability if wi satisfies the normal prior distribution; LASSO regression is estimated with the maximum a posteriori probability if wi satisfies the Laplace prior distribution.

However, both the Ridge regression and LASSO regression suppress the overfitting phenomenon by introducing a penalty term, at the expense of increasing the training error and decreasing the test error. Combining the ideas of these two methods can lead to new optimization methods, which I won't go into here.

Today I have shared with you the basic principles of linear regression, one of the basic machine learning algorithms, the main points of which are as follows.

Linear regression assumes that the output variable is a linear combination of several input variables and solves for the optimal coefficient in the linear combination based on this relationship.
The least squares method can be used to solve univariate linear regression problems and is equivalent to maximum likelihood estimation when the error function obeys a normal distribution.
Multiple linear regression problems can also be solved by least squares, but are highly susceptible to overfitting.
Ridge regression and LASSO regression suppress overfitting by introducing a two-paradigm penalty term and a one-paradigm penalty term, respectively.

