---
title: Optimization
date: 2020-10-01 20:13:02
tags: Mathematics
category: Mathematics
---
Essentially, the goal of AI is optimization: making optimal decisions in complex environments and multi-vessel interactions. Almost all AI problems boil down to solving an optimization problem, so optimization theory is also a fundamental knowledge for AI.

Optimization theory deals with the problem of determining whether the maximum (minimum) value of a given objective function exists, and finding the value that causes the objective function to take the maximum (minimum) value. If a given objective function is viewed as a continuous mountain range, the optimization process is the process of determining the location of a peak and finding a path to reach it.

The function to be minimized or maximized is called the objective function or evaluation function, and most optimization problems can be solved by minimizing the objective function f(x), while maximization problems can be solved by minimizing -f(x).

The difference between the global minimum and the local minimum is that the global minimum is smaller than the function at all other points in the defined domain, whereas the local minimum is only smaller than the function at all neighboring points.

Ideally, the goal of the optimization algorithm is to find the global minimum. But finding the global optimum solution means performing the search on a global scale. Let's take the example of a mountain range. The global minimum corresponds to the highest peak in the mountain range, and the best way to find this peak is to stand at a higher level, take in all the peaks, and then find the highest one among them.

Unfortunately, none of the current practical optimization algorithms have such a view of God. They all stand at the foot of the mountain and look for nearby peaks one step at a time. However, due to vision limitations, the peaks found are likely to be only the tops within a ten-mile radius, i.e., local very small values.

When the objective function has a large number of input parameters and a large solution space, most practical algorithms cannot meet the computational complexity requirements of global search, and thus can only find local very small values. However, in artificial intelligence and deep learning scenarios, as long as the value of the objective function is small enough, the value can be used as a global minimum as a compromise between performance and complexity.

Depending on the constraints, the optimization problem can be divided into two categories: unconstrained optimization and constrained optimization. The unconstrained optimization has no restriction on the value of the independent variable x, while the constrained optimization restricts the value of x to a specific set, that is, to meet certain constraints.

Linear programming is a typical example of constrained optimization, which solves the problem of maximizing the benefits of a finite cost constraint. Constrained optimization problems are usually more complex than unconstrained optimization problems, but problems with n variables and k constraints can be transformed into unconstrained optimization problems with (n+k) variables by the introduction of Lagrange multiplier. In its simplest form, the Lagrange function is as follows.

![](1.jpg)
where f(x,y) is the objective function, φ(x,y) is the equation constraint, and λ is the Lagrange multiplier. In a mathematical sense, the Lagrange function, which is jointly composed of the original objective function and the constraints, has a common set of best merits and a common optimal objective function value, thus ensuring the invariance of the optimal solution.

The most common method for solving unconstrained optimization problems is gradient descent (gradient descent). Intuitively, the gradient descent method is to find the minimum value of the objective function in the direction where it decreases the fastest, like climbing a mountain on the steepest path to the top of the mountain. Mathematically, the direction of the gradient is the opposite direction of the derivative (derivative) of the objective function.

When the input to the function is a vector, the image of the objective function becomes a surface in high-dimensional space, and the gradient is a vector perpendicular to the contour of the surface and pointing in the direction of increasing height, which also carries information about the direction in high-dimensional space. And for the objective function to fall as fast as possible, it is necessary to have the independent variable move in the direction of the negative gradient. This conclusion is translated into mathematical language as "the multivariate function falls fastest in the direction of its negative gradient", which is the theoretical basis of the gradient descent method.

Another important factor in the gradient descent algorithm is the step size, i.e. the value of x that changes with each update of f(x). A small step will result in a slower convergence process, and when f(x) is close to the minimum, a large step will result in a step past the minimum, which is called "too much".

Thus, in gradient descent, the overall rule of step size selection is progressively smaller. This approach is also in accordance with the laws of our understanding. When calibrating an instrument, isn't it always coarse adjustment and then fine adjustment?

The above is the gradient descent method for a single sample, when there are more than one available training samples, the use of samples is divided into two modes.

One is the batch processing mode (batch processing), that is, to calculate the gradient of the objective function in each sample, and then the gradient of the different samples to sum up the results of the sum as the update of the gradient of the objective function. In the batch processing mode, each update traverses all the samples in the training set, thus the computational effort is large.

Another model is called stochastic gradient descent, which uses only one sample in each update and another sample in the next update, traversing all the samples in an iterative update process. Interestingly, random gradient descent has been shown to perform better when the size of the training set is large.

The gradient descent method only uses the first-order derivative of the objective function and does not use the second-order derivative. The first-order derivative describes how the objective function varies with the input, while the second-order derivative describes how the first-order derivative varies with the input, providing information about the curvature of the objective function. The curvature affects the rate of descent of the objective function. When the curvature is positive, the objective function will fall more slowly than expected with gradient descent; conversely, when the curvature is negative, the objective function will fall more rapidly than expected with gradient descent.

The gradient descent method cannot take advantage of the curvature information contained in the second-order derivatives, but only the local properties of the objective function, and thus is inevitably blind in the search. It is known that the objective function may have increasing derivatives in several directions, implying that the descending gradient has multiple choices. However, there are clearly good and bad effects of different choices.

Unfortunately, the gradient descent method does not have access to information about the change in the derivative, and it is not known that the direction in which the derivative is permanently negative should be explored. Without a global view of the objective function, the gradient descent method takes some detours in use, resulting in slower convergence. The global information contained in the second derivative can provide guidance on the direction of gradient descent, which in turn leads to better convergence.

If the second-order derivative is introduced into the optimization process, the typical method obtained is Newton's method. In Newton's method, the objective function is first expanded by Taylor and written in the form of a second-order approximation (in contrast, the gradient descent method preserves only the first-order approximation of the objective function). The second-order approximation is then derived, and its derivative is equal to zero, and the resulting vector represents the direction of the fastest descent. Compared to the gradient descent method, Newton's method converges more quickly.

Whether it is the use of first-order derivatives of the gradient descent method, or the use of second-order derivatives of the Newtonian method, the basic idea of finding the minimum point is to determine the direction, and then determine the step size, thus collectively known as the "linear search" (line search).

There is also a class of algorithms to find the minimum point of the basic idea is to determine the first step length, step length as a parameter to delineate a region, and then in this region to find the fastest direction of decline. This type of algorithm is known as the "trust region method" (trust region).

Specifically, the confidence region algorithm operates as follows: set a confidence radius s, and in the current point as the center, to s as the radius of a closed spherical region as a confidence region, in the confidence region to find the best of the quadratic approximation model of the objective function, the best and the distance between the current point is the calculated alternative displacement.

On the alternative displacement, if the quadratic approximation of the objective function produces a sufficient decline, then the current point is moved to the calculated most meritorious, and the calculation continues iteratively according to this rule, and s can be increased appropriately; if the asymptotic decline of the objective function is not good enough, then the step is too large, and s needs to be reduced and a new alternative displacement calculated until the termination condition is satisfied.

In addition to the above algorithms, there is a class of optimization methods called "heuristics". Inspired by bionics, which was born in the 1950s, heuristics apply the mechanisms of natural phenomena such as biological evolution to the optimization of real-world complex problems, and have achieved remarkable results.

In contrast to traditional mathematical-based optimization methods, heuristics go back to the basics. The core idea behind heuristics is the survival-of-the-fittest rule of nature, with empirical factors such as selection and mutation added to the algorithm's implementation.

In fact, more searches do not necessarily mean more intelligence, but more intelligence is the ability to use heuristics to solve problems without having to do a lot of searching. Examples of heuristic algorithms include genetic algorithms that simulate biological evolution, simulated annealing that simulates the crystallization of solids in statistical physics, and ant colony optimization that produces cluster intelligence in inferior animals.

Neural networks, which are all the rage today, are in fact a class of heuristics that simulate the mechanisms of neuronal competition and collaboration in the brain. If you are interested in these heuristics, you can check out the principles and implementation of different algorithms.

Today I am sharing with you the essential foundations of an optimal approach to AI, focusing on the interpretation of abstract concepts rather than concrete mathematical formulas, the main points of which are as follows.

Typically, the optimization problem is solving for the minimum value of a given objective function in the unconstrained case.

- In linear search, determining the direction of search in finding a minimum requires the use of the first- and second-order derivatives of the objective function.
- The idea behind the confidence domain algorithm is to determine the search step before determining the search direction.
- Heuristic algorithms, represented by artificial neural networks, are another important class of optimization methods.

