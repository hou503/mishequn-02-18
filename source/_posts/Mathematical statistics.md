---
title: Mathematical statistics
date: 2020-10-16 22:02:09
tags: Mathematics
category: Mathematics
---

Mathematical statistics are also indispensable in the study of artificial intelligence. Basic statistical theory helps to interpret the results of machine learning algorithms and data mining, and the value of the data can only be reflected if reasonable interpretations are made. Mathematical statistics (mathematical statistics) is the study of random phenomena based on data obtained from observations or experiments, and makes reasonable estimates and judgments about the objective laws of the object of study.

Although mathematical statistics is based on the theory of probability, there is a fundamental difference in approach between the two. The probability theory is based on the premise that the distribution of random variables is known, and the characteristics and patterns of random variables are analyzed according to the known distribution; the object of mathematical statistics is the unknown distribution of random variables, and the research method is to make repeated independent observations of the random variables and infer the original distribution according to the observation results.

To put it in a less precise but more intuitive way: mathematical statistics can be seen as reverse probability theory. To use the analogy of buying a lottery ticket, probability theory deals with determining the likelihood of a number winning based on the known pattern of the lottery draw, while mathematical statistics deals with guessing the pattern of the lottery draw with a certain degree of accuracy based on a record of previous winning/unwinning numbers, although the attempt is often fruitless.

In mathematical statistics, the available resource is a finite set of data called a sample. Accordingly, all possible values of an observation are called a population. The task of mathematical statistics is to infer the numerical characteristics of the population from the sample. Samples are usually obtained from many independent repeated observations of the population, which ensures that the different sample values are independent of each other and have the same distribution as the population.

In statistical inference, it is often not the sample itself that is applied, but a function of the sample known as the statistic. The statistic itself is a random variable that is used as a tool to make statistical inferences. The sample mean and the sample variance are the two most important statistics.

![](1.jpg)

The basic problems of statistical inference can be divided into two broad categories: estimation theory and hypothesis test.

## Estimation theory
Estimation theory is a method of estimating the overall distribution from randomly selected samples, which can be further divided into point estimation and interval estimation. When the function form of the overall distribution is known, but one or more parameters are not known, estimating the value of the unknown parameter with the help of a sample of the overall distribution is called point estimation of the parameter. The core of point estimation is to construct a suitable statistic θ^ and use the observed value of this statistic as an approximation of the unknown parameter θ. Specific methods of point estimation include the method of moments and maximum likelihood estimation.

Moments represent the characteristics of the distribution of a random variable, and the kth order moment is defined as the mean of the kth power of the random variable, i.e., E(Xk). The idea of moment estimation is to use the k-order moments of the sample to estimate the k-order moments of the total, based on the theory that the function of the sample moment converges almost everywhere to the corresponding function of the total moment, which means that when the sample size is large enough, the sample parameter can be used to get an approximation of the corresponding total parameter almost every time.

As opposed to moment estimation based on the law of large numbers, maximum likelihood estimation stems from the way the frequency school views probability. The intuitive understanding of maximum likelihood estimation is that since the sample is a set of existing sample values, it is assumed that the probability of getting this set of sample values is higher, and therefore the parameter θ needs to be estimated in such a way that the existing sample values have the highest probability of occurring.

In maximum likelihood estimation, the likelihood function is defined as the probability of occurrence of the sample observations, and the criterion for determining the unknown parameter is to maximize the value of the likelihood function, which is the problem of solving the maximum value of the function in calculus. Since the different sample values are independent of each other, the likelihood function can be written as a number of probability mass function / probability density function multiplication, and further transformed into a logarithmic equation for solution.

Moment estimation and maximum likelihood estimation represent two ideas for inferring the overall parameter, but for the same parameter, the estimates obtained by different estimation methods are likely to differ, which raises the question of how to evaluate the estimates. In practice, there are three basic criteria that are usually considered in the evaluation of an estimate.

- Unbiased: the mathematical expectation of the estimated quantity is equal to the true value of the unknown parameter.
- Validity: the variance of the unbiased estimates is as small as possible.
- Consistency: as the sample size approaches infinity, the estimated quantity converges probabilistically to the true value of the unknown parameter.

These three requirements form the overall criteria for determining the point estimate. Unbiased means that given the sample values, the estimates obtained from the estimated quantities may be larger or smaller than the true values. However, if the construction of the estimated quantities is kept unchanged, and instead multiple resamplings are performed, each time with a new sample to calculate the estimated values, then the deviation of these estimated values from the true values of the unknown parameters is equal to zero in an average sense, which means that there is no systematic error.

Although deviations between the estimated and true values are unavoidable, the smaller the deviation in an individual sense means that the performance of the estimate is more accurate, and validity measures precisely the degree of deviation between the estimated and true values. The degree of deviation depends not only on how the estimate is constructed, but also on the size of the sample size, and consistency considers the effect of sample size. Consistency indicates that as the sample size increases, the value of the estimated quantity will stabilize at the true value of the unknown parameter. Estimates that do not have consistency will never estimate the unknown parameter with sufficient accuracy, and are therefore undesirable.

The criterion for discriminating the estimated quantity involves the effect of the estimation error, which is an equally important parameter as the estimated value. In the process of estimating the unknown parameter θ, in addition to deriving an estimate, an interval needs to be estimated and the degree of confidence that this interval contains the true value of θ needs to be determined. In mathematical statistics, this interval is called the confidence interval, and this type of estimation is called interval estimation.

Confidence intervals can be interpreted visually in the following way: If the total population is sampled repeatedly, each time with the same sample size, a confidence interval (θ-,θ¯) can be determined from each set of sample values, with the upper and lower bounds being the two statistics of the sample, representing the upper and lower confidence limits, respectively.

Two possibilities exist for each confidence interval: a true value that contains θ or a true value that does not. If you count the ratio of all confidence intervals that contain the true value of θ, the resulting ratio is the confidence level. Therefore, the interval estimation provides a further range and error limit on top of the point estimate, corresponding to the confidence interval and the confidence level, respectively.

## Hypothesis test

The object of estimation theory is some parameter of the aggregate, while the object of hypothesis test is some assertion about the aggregate, i.e., a hypothesis about the aggregate. The hypothesis in a hypothesis test contains the original hypothesis H0 and the alternative hypothesis H1; the test is the process of choosing an acceptance between H0 and H1 based on the sample.

Ideally, the hypothesis H0(H1) should be assumed to be true and the hypothesis should be accepted. However, since the test is based on samples, false decisions will eventually occur, and they can be of two types: Type I errors correspond to cases where H0 is true but rejected, i.e., "discarded" errors; Type II errors correspond to cases where H0 is untrue but accepted, i.e., "rejected" errors. A "falsification" type of error.

The hypothesis-testing mindset is based on the idea that full propositions can only be falsified but not proven. The easier way to prove that the original hypothesis H0 is true is to prove that the alternative hypothesis H1 is false, since it is enough to be able to cite a counterexample. In hypothesis test, however, counterexamples are not violations of the hypothesis in an absolute sense, but rather in the form of small probability events.

In mathematical statistics, an event with a probability of less than 1% is called a small probability event and is considered unlikely to occur in a single experiment. If a small probability event occurs in a sample obtained from a single observation, then it is reasonable to assume that it is not really a small probability event, and the original assumption is thus overturned. If the alternative hypothesis is overturned, it implies acceptance of the original hypothesis; conversely, if the original hypothesis is overturned, it implies rejection of the original hypothesis.

From a mathematical and statistical point of view, the task of a supervised learning algorithm is to search the hypothesis space for hypotheses that make good predictions for a given problem. The ability of the learner to learn from the test dataset to obtain a model with generalizability that is applicable to new samples that are not part of the test set is called generalizability. Obviously, the better the generalization ability, the better the learner is.

The role of hypothesis test is to infer the strength of the generalization ability of the learner based on its performance on the test set and to determine the accuracy of the conclusions obtained, which can be further extended to compare the performance of different learners. Since a common measure of learner performance is the error rate, the hypothesis in hypothesis test is to infer the generalized error rate of the learner, and the inference is based on the test error rate on the test data set. There are many different ways to test this, and I will not go into them here.

In addition to inference, the interpretation of generalization performance is also an important part of the analysis of machine learning algorithms. The composition of generalization error can be divided into three parts: bias, variance, and noise.

Bias represents the deviation between the predicted value of the algorithm and the true result, depicting the underfitting characteristics of the model; variance represents the effect of the perturbation of the data on the prediction performance, depicting the overfitting characteristics of the model; and noise represents the minimum generalization error that can be achieved on the current learning task, depicting the difficulty of the task itself. For any realistic model, bias and variance are difficult to optimize simultaneously, reflecting the irreconcilable conflict between underfitting and overfitting.


Today I share with you the essential mathematical and statistical foundations of AI, focusing on the interpretation of abstract concepts rather than concrete mathematical formulas, the key points of which are as follows.

- The task of mathematical statistics is to infer the nature of the aggregate from an observable sample in turn.
- The instrument of inference is the statistic, which is a function of the sample and is a random variable.
- Estimation theory estimates unknown parameters of the overall distribution through randomly selected samples, including point estimates and interval estimates.
- Hypothesis test accepts or rejects a certain judgment about the aggregate by a randomly selected sample and is commonly used to estimate the generalized error rate of machine learning models.

