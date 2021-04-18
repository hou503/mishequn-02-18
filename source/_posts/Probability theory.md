---
title: Probability theory
date: 2020-09-23 19:31:19
tags: Mathematics
category: Mathematics
---
In addition to linear algebra, probability theory (PROBABILITY) is an essential mathematical foundation for AI research. With the rise of the connectionist school of thought, probability statistics has replaced mathematical logic as the mainstream tool for AI research. In today's world of exploding data and exponentially increasing computing power, probability theory has taken on a central role in machine learning.

Like linear algebra, probability theory represents a way of looking at the world that focuses on ubiquitous possibilities. The specification of a mathematical description of the probability of a random event occurring is the axiomatization process of probability theory. The axiomatized structure of probability embodies an understanding of the nature of probability.

When the same coin is tossed 10 times, it may be face-up either none or all of the times, which translates into frequencies corresponding to 0% and 100%, respectively. The frequency itself will obviously fluctuate randomly, but as the number of repetitions increases, the value of the frequency of a particular event will show stability and converge to a constant.

The method of understanding probability from the frequency of an event is called "frequencyentist probability", and the "probability" in frequencyist parlance is actually a single result in a random experiment that can be repeated independently. Limits of frequency of occurrence. Because a stable frequency is an expression of statistical regularity, it is a reasonable idea to calculate the frequency through a large number of independent repeated trials and use it to characterize the probability of an event occurring.

In the quantitative calculation of probability, the frequency school relies on the basis of the classical probability model. In the classical probabilistic model, the outcome of a trial contains only a finite number of fundamental events, each of which has the same probability of occurring. In this way, assuming that the number of all elementary events is n and the number of elementary events contained in the random event A to be observed is k, the formula for the probability of an event under the classical probability model is as follows

![](1.jpg)

The probability of complex random events can be derived from this basic formula.

The previous definition of probability is for a single random event, but it's not enough to describe the relationship between two random events. In a soccer match, the probability of a team winning 1:0 is obviously not the same as the probability of being down 0:2 and overturning the game 3:2. This brings us to the concept of conditional probability.

Conditional probability is a new probability distribution obtained by adjusting the sample space based on existing information. Assuming two random events A and B, conditional probability is the probability that event A will occur under the conditions that event B has already occurred, expressed by the following equation

![](2.jpg)

P(AB) in the above equation is called joint probability, which represents the probability of two events A and B occurring together. If the joint probability is equal to the product of the probabilities of the two events, i.e. P(AB) = P(A)⋅P(B), it means that the occurrence of these two events does not affect each other, i.e. they are independent of each other. For mutually independent events, the conditional probability is the probability of itself, i.e., P(A|B) = P(A).

Based on the conditional probability, the law of total probability formula can be derived. The function of the total probability formula is to transform the probability solution of a complex event into a summation of the probabilities of simple events occurring under different circumstances, i.e.

![](3.jpg)
The full probability formula represents the frequency school's approach to solving probability problems by making some assumptions (P(Bi)) and then discussing the probability of a random event (P(A|Bi)) under these assumptions.

The inverse probability problem is an important one, which is solved by slightly tidying up the full probability formula. Inverse probability solves the problem of inferring the probability of various hypotheses (P(Bi|A)), given that the outcome of the event is certain (P(A)). Since this theory was first developed by the English clergyman Thomas Bayes, its common form is known as the Bayesian formula.

![](4.jpg)

Bayes' formula can be further abstracted into Bayes' theorem (Bayes' theorem).

![](5.jpg)

where P(H) is referred to as prior probability, the probability that a predetermined hypothesis holds; P(D|H) is referred to as likelihood function, the probability that an outcome is observed if the hypothesis holds; and P(H|D) is referred to as posterior probability, the probability that an outcome is observed if the hypothesis holds.

In terms of scientific research methodology, Bayes' theorem provides an entirely new logic. It looks for reasonable hypotheses based on observations, or the best theoretical explanation based on observations, and its focus is on posterior probability. The Bayesian school of probability was born out of this idea.

In the Bayesian view, probability describes how plausible a random event is. If the weather app on your phone gives an 85% probability that it will rain tomorrow, this cannot be explained in terms of frequency, but means that the probability of it raining tomorrow is 85%.

The frequency school considers the assumption to be objective and unchanging, i.e. there is a fixed a priori distribution, but we as observers have no way of knowing it. Therefore, when calculating the probability of a specific event, we need to determine the type and parameters of the probability distribution, and use this as the basis for probabilistic deduction.

The Bayesian school, by contrast, holds that a fixed prior distribution does not exist and that the parameters themselves are random numbers. In other words, the hypotheses themselves depend on observations, are uncertain and subject to revision. The role of data is to make constant revisions to the assumptions, bringing the observer's subjective perception of probability closer to objective reality.

Probability theory is another theoretical basis for artificial intelligence in addition to linear algebra, and most machine learning models use a probabilistic-based approach. However, due to the limited training data available for practical tasks, the parameters of the probability distribution need to be estimated, which is the core task of machine learning.

There are two approaches to probability estimation: maximum likelihood estimation and maximum a posteriori estimation, both of which reflect the frequency and Bayesian ways of understanding probability, respectively.

The idea of maximum likelihood estimation is to maximize the probability of the training data, determine the unknown parameters in the probability distribution accordingly, and the estimated probability distribution will be the most consistent with the distribution of the training data. The idea of the maximum a posteriori likelihood method, on the other hand, is to maximize the probability of the occurrence of the unknown parameter based on the training data and other known conditions, and to select the most likely value of the unknown parameter taken as an estimate. In estimating the parameters, the maximum likelihood estimation method only requires the use of training data, the maximum a posteriori probability method requires additional information in addition to the data, which is the prior probability in the Bayesian formula.

From a theoretical point of view, the frequency school and the Bayesian school each play an irreplaceable role. However, specifically in the application field of artificial intelligence, the various methods based on Bayes' theorem are more closely aligned with human cognitive mechanisms and play a more important role in fields such as machine learning.

An important application of probability theory is the description of random variables. According to the different value spaces, random variables can be divided into two categories: discrete random variable and continuous random variable. In practice, it is necessary to describe the probability of each possible value of a random variable.

Each possible value of a discrete variable has a probability greater than 0. The one-to-one correspondence between values and probabilities is the law of distribution of discrete random variables, also known as the probability mass function. Probability mass function in the continuous random variables on the correspondence is the probability density function (probability density function).

It should be noted that the probability density function reflects not the true probability of continuous random variables, but the relative relationship between the different possibilities. For continuous random variables, the number of possible values for the infinite can not be listed, when the probability of normalization is assigned to the infinite points, the probability of each point is an infinitesimal amount, take the limit is equal to zero. The role of the probability density function is to distinguish between these infinitesimal quantities. Although 1/x and 2/x are both infinitesimal at x → ∞, the latter is always twice as large as the former. This kind of difference in relative rather than absolute terms can be depicted by the probability density function. Integrating the probability density function yields the probability that the value of a continuous random variable will fall within a certain interval.

After defining the probability mass function and the probability density function, it is possible to give some characteristics of the important distributions. Important discrete distributions include two-point, binomial, and Poisson distributions, while important continuous distributions include uniform, exponential, and normal distributions.

- Bernoulli distribution: Applies to the case where the outcome of a random trial is binary and the probability of the event occurring/not occurring is p/(1-p). Any random trial with only two outcomes can be described by a two-point distribution, and the result of a coin toss can be considered a two-point distribution with equal probability.

- Binomial distribution: The two-point distribution of random trials satisfying the parameter p is repeated n times independently, and the number of events is the binomial distribution satisfying the parameter (n,p). The expression for the binomial distribution can be written as P(X=k)=Cnk⋅pk⋅(1-p)(n-k),0≤k≤n.
- Poisson distribution: The distribution satisfied by the number of particles released from radioactive material in the specified time. When n is large in the binomial distribution and p is small, the probability value can be approximated by the probability value of the Poisson distribution with λ=np.
- Uniform distribution: A continuous random variable that satisfies a uniform distribution over an interval (a, b), with probability density function 1 / (b - a), which has equal probability of falling within any subinterval of equal length within the interval (a, b).
- Exponential distribution: a random variable with parameter θ can only take positive value and its probability density function is e-x/θ/θ,x>0. An important feature of exponential distribution is that it has no memory: P(X > s + t | X > s) = P (X > t).
- Normal distribution: the probability density function of a normal distribution with parameters as

![](6.jpg)

When μ=0,σ=1, the above equation is called the standard normal distribution. The normal distribution is the most common and important type of distribution, and many phenomena in nature approximately obey the normal distribution.

In addition to the probability mass function / probability density function, another class of parameters that describe random variables are their numerical characteristics. Numerical characteristics are constants used to portray certain characteristics of random variables, including expected value, variance, and covariance.

Mathematical expectations, or mean values, represent the weighted average of the possible values of a random variable, i.e., the pattern that describes the random variable as a whole according to the probability of each value occurring. The variance is the degree to which the values of the random variables deviate from their mathematical expectation. Smaller variance means that the values of the random variables are concentrated near the mathematical expectation, while larger variance means that the values of the random variables are more scattered.

Both mathematical expectations and variance describe the numerical characteristics of a single random variable, and covariance and correlation coefficients are used to describe the interrelationship between two random variables. The covariance measures the linear correlation between two random variables, i.e. whether the variable Y can be expressed as aX+b with the other variable X as the dependent variable.

The correlation coefficient is a constant with an absolute value not greater than 1, equal to 1 means that the two random variables are perfectly positively correlated, equal to -1 means that they are perfectly negatively correlated, and equal to 0 means that they are not correlated. It is important to note that both covariance and correlation coefficients portray a linear correlation. If the relationship between the random variables satisfies Y=X2, such a non-linear correlation is beyond the expressive power of the covariance.


Today I share with you the essential probabilistic foundations of AI, focusing on the interpretation of abstract concepts rather than concrete mathematical formulas, the key points of which are as follows.

- Probability theory is concerned with the uncertainties or possibilities of life.
(a) The frequency school considers the a priori distribution to be fixed and the model parameters to be calculated by maximum likelihood estimation.
- Bayesian school of thought that a priori distributions are stochastic and that model parameters are calculated by maximizing a posteriori probabilities.
- Normal distribution is the most important kind of distribution of random variables.

In today's machine learning, a great deal of the task is to predict what is likely to happen based on existing data, hence Bayes' theorem is widely used.

