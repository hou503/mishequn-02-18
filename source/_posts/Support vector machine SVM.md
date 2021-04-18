---
title: Support vector machine SVM
date: 2020-12-04 19:21:51
tags: Algorithm
category: Algorithm
---

### What is the principle of SVM?

　　SVM is a two-class classification model. Its basic model is a linear classifier that finds the separated hyperplane with maximized interval in the feature space. (Interval maximization is what distinguishes it from a perceptron) 
It tries to find a hyperplane to segment the sample, separating the positive and negative examples in the sample with a hyperplane and maximizing the interval between the positive and negative examples as much as possible.

　　The basic idea of support vector machine can be summarized as, firstly, transforming the input space to a higher dimensional space by a nonlinear transformation, and then finding the optimal classification plane in this new space, i.e., the maximum interval classification plane, and this nonlinear transformation is achieved by defining an appropriate inner product kernel function.SVM is actually proposed according to the statistical learning theory in accordance with the principle of minimizing structural risk, which requires to achieve two purposes the separation of the two types of problems 

(1) separation of the two types of problems (empirical risk minimization)

(2) maximizing margin (minimizing the upper bound on risk) both by choosing the function with the lowest empirical risk in the subset of guaranteed risk minimization.

Divided into 3 classes of support vector machines. 
(1) When the training samples are linearly divisible, a linear classifier is learned by hard interval maximization, i.e., a linearly divisible support vector machine.

(2) When the training data are approximately linearly divisible, a linear classifier, i.e., linear support vector machine, is learned by soft interval maximization by introducing relaxation variables.

(3) When the training data is linearly indistinguishable, a nonlinear support vector machine is learned by using the kernel trick and soft interval maximization.

Note: The mathematical derivation of each of the above SVMs should be familiar with: hard interval maximization (geometric interval) - learned pairwise problem - soft interval maximization (introduction of relaxation variables) - nonlinear support vector machine (kernel trick).

### Main features of SVM

(1) Nonlinear mapping - theoretical basis 

(2) Maximization of classification boundary - method core 

(3) Support vector - computational results 

(4) Small sample learning method

(5) The final decision function is determined by only a small number of support vectors, avoiding the "dimensional disaster"

(6) A small number of support vectors determines the final result --> a large number of redundant samples can be "eliminated" + the algorithm is simple + has robustness (reflected in 3 aspects)

(7) Learning problem can be expressed as a convex optimization problem --> global minimum

(8) can automatically control the model by maximizing the boundary, but requires the user to specify the type of kernel function and the introduction of relaxation variables

(9) Suitable for small samples, excellent generalization capability (because of minimal structural risk) (10) Low generalization error rate, fast classification, and easy to interpret results.

Disadvantages: 
(1) Large-scale training samples (m-order matrix computation) 

(2) Traditionally not suitable for multi-classification 

(3) Sensitive to missing data, parameters, kernel functions

Why is SVM sensitive to missing data?

Missing data here means that some feature data is missing and the vector data is incomplete. sVM does not have a strategy to deal with missing values (decision tree does). And SVM expects the samples to be linearly separable in the feature space, so the goodness of the feature space is important to the performance of SVM. Missing feature data will affect the goodness of the training results.

### Why does SVM use interval maximization?

When the training data is linearly separable, there exist infinite separating hyperplanes to correctly separate the two types of data. The perceptual machine uses the misclassification minimization strategy to find the separation hyperplane, but there are infinite number of solutions at this time.

Linearly separable support vector machines use interval maximization to find the optimal separation hyperplane, at which point the solution is unique. On the other hand, the separation hyperplane at this point produces the most robust classification result, with the best generalization to unknown instances.

This should then be used to elaborate on the geometric interval, the functional interval, and the w and b when minimizing 1/2 ||w||^2 from the functional interval->solution. i.e., the origin of the linearly separable support vector machine learning algorithm-maximum interval method.

What is maximized and what is minimized in svm?

In various explanations of SVM, there is one knowledge point that is not well covered: the objective function of SVM is to maximize the geometric interval of the support vector, but how does it end up minimizing the normal vector (slope)?        

Imagine a hyperplane where the slope and intercept increase by the same multiple, and the hyperplane is constant. In other words, the parameters of a fixed hyperplane are not fixed. When we find the optimal hyperplane, the solution space becomes infinite. We can of course reduce the solution space by setting some constraints on these parameters in advance. Then, this constraint is: make the function interval of the support vector = 1.       

The advantage of this constraint is twofold: In the case that the hyperplane is not determined, of course no one knows which vectors the support vectors are, and there is only a formal expression for the geometric interval of the support vectors, not to mention how to express "maximize the geometric interval of the support vectors". But with the above constraint, it does not matter who is the support vector in the expression of "geometric interval of support vector", the only part related to the sample, that is, the function interval, has become 1. The function interval of other samples should be larger than the function interval of support vector, which is the only constraint to be satisfied. At this point, the solution space of the problem is no longer infinite, and there is a meaningful solution space.

Support vector regression essentially has nothing to do with SVM, and the name is more confusing. However, it is included in libSVM, so I have to talk about it. In fact, it is solving a linear regression problem, but due to the increase of the minimum parametric requirement for the slope, the optimization problem formally resembles SVM, and the final expression of the linear function is also very similar to SVM, appearing in the form of a wonderful inner product with the support vector.

### Why should we convert the original problem of solving SVM to its dual problem?

One, is that the dual problem is often easier to solve (when we look for the best advantage when the constraint exists, the existence of the constraint reduces the scope of the search required, but makes the problem more complex. (To make the problem manageable, our approach is to incorporate the objective function and constraints into a new function, the Lagrangian function, and then find the optimal point through this function.) 

Note: Lagrangian pairing does not change the optimal solution, but changes the complexity of the algorithm: original problem - sample dimension; pairing problem - number of samples. So linear classification -> sample dimension < sample number: original problem solved (liblinear default); nonlinear - up-dimensional -> general leads to sample dimension > sample number: pairwise problem solved.

Second, naturally introduce kernel functions, which in turn extend to nonlinear classification problems.

### Explain the support vector

Definition in the linearly divisible case + definition in the linearly indivisible case . (Statistical learning methods) 

(1) Several equivalent definitions of SV for linear separable SVM 

(2) Several equivalent definitions of SV for linear SVM 

(3) Compare the difference and connection between the definition of SV for linear separable SVM and the definition of SV for linear SVM

Why do SVMs introduce kernel functions?

When the sample is linearly indistinguishable in the original space, the sample can be mapped from the original space to a higher dimensional feature space, making the sample linearly distinguishable in this feature space.

Pairwise problems after the introduction of mapping. 

![1.png](1.png)

In learning prediction, only the kernel function K(x,y) is defined instead of explicitly defining the mapping function ϕ. Because the feature space dimension may be high, possibly even infinite, it is more difficult to compute ϕ(x)-ϕ(y) directly. In contrast, it is easier to compute K(x,y) directly (i.e., directly in the original low-dimensional space without explicitly writing out the result of the mapping).

The kernel function is defined by K(x,y)=<ϕ(x),ϕ(y)>, i.e., the inner product in the feature space is equal to the result they compute in the original sample space by the kernel function K.

Except for SVM, any method that represents the computation as an inner product of data points can be extended nonlinearly using the kernel method.

### What is the specific formula for the svm RBF kernel function? (Gaussian kernel function, also called Radial Basis Function (RBF for short). It can map the original features to infinite dimensions)


![2.png](2.png)

Advantages of RBF kernels. 
It is applicable to both large and small. Specifically (1) infinite-dimensional, linear kernel is its special case (2) compared to polynomial ~, RBF requires fewer parameters to be determined (3) under certain parameters, it has a similar function to sigmoid ~.

The Gauss radial basis function, on the other hand, is a strongly localized kernel function whose extrapolation ability decreases with increasing parameter σ.

This kernel will map the original space to an infinite dimensional space. However, if σ is chosen to be very large, the weights on the higher order features actually decay very fast, so that it is actually (to approximate numerically) equivalent to a low-dimensional subspace; conversely, if σ is chosen to be very small, it can map arbitrary data to be linearly divisible - which, of course, is not necessarily a good thing, because very serious overfitting problems may follow. However, in general, by tuning the parameter σ , the Gaussian kernel is actually quite flexible and one of the most widely used kernel functions.

### How does SVM handle multi-classification problems?

There are two general approaches: one is the direct method, which directly modifies the objective function and combines the parameter solutions of multiple classification surfaces into one optimization problem. This seems to be simple, but the computational effort is very large.

The other approach is the indirect approach: combining the trainers. The typical ones are one-to-one, and one-to-many.

One-to-many is to train a classifier for each class. Since svm is binary, the two classes of this binary classifier are set as one class for the target class and another class for the remaining classes. In this way, k classifiers can be trained for k classes, and when a new sample comes in, the k classifiers are used to test which class the sample belongs to if the classifier has a high probability. This method is not very effective and the bias is relatively high.

If there are k classes, a total of C(2,k) classifiers are trained, so that when a new sample comes, the C(2,k) classifiers are used to test, and whenever a class is determined to belong to a class, the class is added one, and the class with the most votes is determined to be the class of the sample. 

### Differences and connections between SVM and LR

Connection:

(1) classification (binary classification) 

(2) can be added to the regularization term 

Differences: 
(1) LR-parametric model; SVM-non-parametric model? 

(2) Objective function: LR-logistical loss; SVM-hinge loss 

(3) SVM-support vectors; LR-reduce the weights of the farther point 

(4) LR -model is simple, well understood, low accuracy, may be locally optimal; SVM-understanding, optimization complex, high accuracy, global optimal, transformed into a pairwise problem -> simplify the model and computation 

(5) LR can do what SVM can do (linearly divisible), SVM can do what LR may not be able to do (linearly indistinguishable)

The relationship between kernel function selection and FEATURE, sample.

(1) fea large ≈ the number of samples: LR or linear kernel 

(2) fea small, the number of samples is not small nor small: Gaussian kernel 

(3) fea large, the number of samples: manually add features after turning

Kernel function is actually a similarity measure of the input data, the input vectors form a similarity matrix K (Gram Matrix/Similarity/Kernel Matrix), K is symmetric semi-positive definite.

The sufficient condition for K(x,z) to be a positive definite kernel is that the Gram matrix corresponding to K(x,z) is a real semi-positive definite matrix. 

Gram matrix: inner product of the points corresponding to the matrix. ktk, kkt 

Semi-positive definite matrix: let A be a real symmetric matrix. A is said to be semi-positive definite if for any real non-zero column matrix X there is XTAX ≥ 0. 

When testing whether a K is a positive definite kernel function, verify whether the real Gram matrix corresponding to K is a semi-positive definite matrix for any finite set of inputs {xi...}.

