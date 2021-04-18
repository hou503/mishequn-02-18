---
title: Backward feed-back neural network algorithm (BP algorithm)
date: 2020-11-21 09:32:56
tags: Algorithm
category: Algorithm
---


## Principle of BP algorithm

The BP algorithm consists of an input layer → hidden layer → output layer.

![WX20210104-083124.png](WX20210104-083124.png)

Input-output layer: Here you can see that this layer can have multiple entries or only 1 entry.

For example, to make a prediction of the next period's result based on the results of each welfare lottery.

Then the input layer should be the result of the previous period (N numbers). The output layer is this period (N numbers).

Hidden layer: The hidden layer can have N nodes or N levels.

Forward process.

The input layer is accumulated to each node of the hidden layer separately 

Suppose the input is x and the node of the hidden layer is s

then

s1 = x1+x2+x3+x4+......

s2 = x1+x2+x3+x4+......

s3 = x1+x2+x3+x4+......

 

Then in order to be able to dynamically adjust the weights added to each node w1 w2 w3 ......

it becomes

 

s1 = w1*x1+w2*x2+w3*x3+w4*x4+......

s2 = w1*x1+w2*x2+w3*x3+w4*x4+......

s3 = w1*x1+w2*x2+w3*x3+w4*x4+......

Adding an offset value b by the way the mathematical formula becomes

![194179-20161121165054581-1673560744.png](194179-20161121165054581-1673560744.png)

Sj also has to go through the transfer function f() to calculate the value of the hidden node

The f() transfer function corresponds to the process by which the

neuron accumulation of biocurrent reaches a certain level triggers the discharge. If the que value is not reached, the accumulated bioelectricity is lost.

As an example 

A commonly used excitation function is the sigmoid function diagram as follows

Equation f= 1/(1+e^-x)

![194179-20161121170525425-1105984505.png](194179-20161121170525425-1105984505.png)

The main significance of adding the sigmod function to the BP algorithm is to add a nonlinear function to solve the nonlinear problem.

The advantage of choosing sigmod is that it is convenient to find the derivative of sigmod = 1 - f 

The derivative needs to be computed for the backward feedbacks

The process is the same for the hidden layer to the output layer.

But the focus of the BP algorithm is on the backward feedbacks.

Reverse.

After the forward transfer is finished we are able to get the result once.

This result can then be compared with the expected value, and the variance E is generally calculated.

Then this error can be passed backwards to the upper layer and used to adjust the weights w of the nodes in the upper layer. This process is repeated until the variance E is less than the desired minimum error. (Because it is impossible to achieve 0 error in reality, the program cannot be ended without setting the minimum desired error.)

As for the specific weight adjustment formula has a complete derivation process, too complex to discuss here.

Translated with www.DeepL.com/Translator (free version)

![194179-20161121173615159-1847785388.png](194179-20161121173615159-1847785388.png)

The core concept is to calculate the partial differentiation of the overall variance E on the weight variable Wj to derive whether the current weight should be increased or decreased, and the letter n in the above equation is the learning rate. It is the basic unit used here to adjust the weights at a time.

Partial differentiation.

For example, a formula with N variables y = aX +bY+cZ; where XYZ is a variable. abc is a constant.

A partial differential is the effect of a change in a single variable on the value of y. The other variables are treated as constants here for derivative purposes.

Then y should be =a ; (bY+CZ) is treated as a constant here to find the partial differential of x.

 

Summary.

Then one forward feedback + one reverse adjustment of the weights makes the global error decrease, and do it several more times until the global error meets the desired minimum error. This is one training completion.

Multiple training requires providing different input values.

Taking the above example again.

That is, I take the lottery results of period 1 as input values and the lottery results of period 2 as expected values to calculate the error, which is a complete training.

The second training is to take the second period as input and the third period as expected value.

And so on until there are no new training samples. The trained neural network can then be used to predict the outcome of any future lottery draw.

(Of course, since welfare lotteries are completely random and irregular, the prediction results are actually very bad.)

The BP algorithm is theoretically able to approximate all linear functions.

In layman's terms, BP algorithms can be trained to predict results when you have enough historical data and don't know the pattern.

But there must be a pattern to what you are trying to do (you just don't know or it's too complicated), and your training set must contain all "valid factors".

I made up the term "valid factors". Let's say that 50% of your historical data affects the results, and the other 50% has nothing to do with the results. This is allowed by the neural algorithm.

But if that 50% of the data does not contain all the factors that lead to the outcome, then the results will vary greatly.

Let's say the welfare lottery (here we go again) has a factor of air quality (hypothetical!) , for example the probability of a certain number appearing without haze is high.

However you do not include this factor in your training set, then the results are very inaccurate.


## Current Problems

There are two obvious problems with the current BP algorithm:

1. easy to fall into local minima

For example, the following figure.

![3.jpg](3.jpg)

The whole BP algorithm is similar to solving for the minimum of this function.

But we see that this function has 2 minima, and in reality there can be very many minima.

When we adjust the weights from right to left, we reach the first trough. Then after continuing to adjust the weights we find that the error value becomes larger and the algorithm mistakenly thinks it has found the minimum solution. However, it does not get the global optimal solution.

This is called getting into a local minimum.

2. Overfitting problem
Overfitting the training set. Like I just gave an example that 50% of the training data is irrelevant to the outcome. So the best result of the algorithm is actually to ignore this 50% useless training set. But the problem is that we don't know what the law is, and we don't know which training set is the valid factor (if we knew, we wouldn't have come up with a BP algorithm).

So over-conforming the training set is also wrong.

