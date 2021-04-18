---
title: Introduction to Machine Learning
date: 2020-11-07 21:28:45
tags: AI
category: AI
---

I don't know if you have ever noticed in your life that we can easily distinguish Japanese, Koreans and Thais by their looks, but we are blind to the faces of English, Russians and Germans. The reason for this is that, on the one hand, Japan, Korea, and Thailand are our neighbors and we have more opportunities to observe ordinary people in these countries; on the other hand, regardless of clothing and make-up, the same people make it easier to compare and distinguish facial features.

Therefore, a lot of observations can lead to the conclusion that Chinese people have moderate jaws, Japanese people have long faces and long noses, Koreans have small eyes and high cheekbones, and Thais have dark complexions. It is these characteristics that are used as the basis for determining whether passerby A is from Japan or passerby B is from Korea.

The above example is a simplified version of the human learning mechanism: extracting recurring patterns and motifs from a large number of phenomena. The implementation of this process in artificial intelligence is machine learning.

Defined formally, an algorithm is said to implement machine learning if it uses some experience to improve its own performance on a particular class of tasks. And from a methodological point of view, machine learning is the discipline in which computers build probabilistic statistical models based on data and use the models to predict and analyze the data.

Machine learning can be described as coming from the data and going to the data. Assuming that the existing data has certain statistical characteristics, the different data can be considered as samples that meet the same distribution independently. What machine learning does is to derive a model describing all the data based on the existing training data and to achieve optimal prediction of the unknown test data based on the derived model.

In machine learning, data are not quantitative values in the usual sense, but descriptions of certain properties of objects. The described properties are called attributes, the values of the attributes are called attribute values, and the vectors obtained from the orderly arrangement of the different attribute values are the data, also called instances.

In the example at the beginning of this article, the typical attributes of a yellow face include skin color, eye size, nose length, and cheekbone height. The standard Chinese example A is a combination of the attributes {light, large, short, low }, while the standard Korean example B is a combination of the attributes {light, small, long, high }.

According to the knowledge of linear algebra, the different attributes of the data can be considered independent of each other, and thus each attribute represents a different dimension that together tensor the feature space.

Each set of attribute values is a point in this space, and thus each instance can be considered a vector, or feature vector, in the feature space.

It is important to note that the feature vector here is not the one corresponding to the feature value, but the vector in the feature space. The output can be obtained by classifying the input data according to the feature vector.

In the previous example, the input data is a person's facial features, and the output data is a Chinese / Japanese / Korean / Thai person out of four. In the actual machine learning task, the form of output may be more complex. Depending on the type of input and output, the prediction problem can be divided into the following three categories.

1. Categorical problems: the output variable is a finite number of discrete variables, the simplest bicategorical problem when the number of variables is 2.

2. Regression problems: both input and output variables are continuous variables.

3. Labeling problem: Both input and output variables are sequences of variables.


In reality, however, people from every country are not the same and naturally look very different, so a Korean with thick eyebrows and big eyes could be mistaken for a Chinese, or a Japanese with darker skin could be mistaken for a Thai.

The same problem can exist in machine learning. It is impossible for an algorithm to match all the training data exactly, nor can it predict all the test data accurately. Therefore, error performance becomes one of the important metrics in machine learning.

In machine learning, error is defined as the difference between the actual predicted output of the learner and the true output of the sample. In classification problems, the common error function is the error rate, which is the proportion of all samples that are classified incorrectly.

Errors can be further divided into two categories: training errors and test errors. Training error refers to the error of the learner on the training dataset, also known as empirical error; test error refers to the error of the learner on the new sample, also known as generalization error.

The training error describes the correlation between the input attributes and the output classification and can determine whether a given problem is an easy one to learn. Test error, on the other hand, reflects the ability of the learner to predict an unknown test data set and is an important concept in machine learning. Practical learners are those that have low test error, i.e., they perform better on new samples.

Learners rely on known data to fit the real situation, i.e., the model obtained by the learner should be as close to the real model as possible, and therefore extract universal laws that apply to all unknown data in the training dataset as much as possible.

However, once too much emphasis is placed on the training error and the degree of conformity between the prediction laws and the training data is pursued, some of the non-universal properties of the training sample itself will be mistaken for the universal nature of all data, leading to a decrease in the generalization ability of the learner.

In the preceding example, if there are few foreigners in contact with the training data and no Koreans with double eyelids, the erroneous stereotype of "single eyelids are all Koreans" will inevitably appear in one's thinking, which is a typical overfitting phenomenon that mistakes the features of the training data for the features of the whole.

The reason for overfitting is usually that the model contains too many parameters during learning, resulting in low training errors but high testing errors.

The counterpart to overfitting is underfitting. If the cause of overfitting is too much learning, the cause of underfitting is too little learning, so that the basic properties of the training data are not learned. The consequence of underfitting is that the learner may even mistake an image of a chimpanzee for a human if the learner's capabilities are inadequate.

In practical machine learning, underfitting can be overcome by improving the learner's algorithm, but overfitting cannot be avoided, and its effects can only be minimized. Since the number of training samples is limited, a model with a finite number of parameters is sufficient to include all training samples.

But the more parameters the model has, the less data there is to accurately match the model, and when such a model is applied to an infinite amount of unknown data, overfitting is unavoidable. Moreover, the training sample itself may contain some noise, and this random noise will bring additional errors to the accuracy of the model.

On the whole, the relationship between test error and model complexity is parabolic. When the complexity of the model is low, the test error is high; as the complexity of the model increases, the test error will gradually decrease and reach a minimum value; then as the complexity of the model continues to increase, the test error will then increase, corresponding to the occurrence of overfitting.

In model selection, in order to make a more accurate estimate of the test error, a widely used method is cross-validation. The idea of cross-validation lies in the repeated use of a limited number of training samples, by cutting the data into subsets, so that different subsets make up the training set and test set respectively, and on this basis, repeated training, testing and model selection to achieve the optimal effect.

If the training data set is divided into 10 subsets D1-10 for cross-validation, each model needs to be trained in 10 rounds, where the training set used in round 1 is the 9 subsets D2~D10, and the trained learners are tested on subset D1; the training set used in round 2 is D1 and The 9 subsets D3~D10, the trained learners are tested on subset D2. And so on, when the model completes testing on all 10 subsets, its performance is the average of the 10 test results. The model with the lowest average test error among the different models is also the optimal model.

In addition to the algorithm itself, the value of the parameters is also an important factor that affects the performance of the model, the same learning algorithm in different parameter configurations, the performance of the model will be significantly different. Therefore, parameter tuning, i.e., setting the parameters of an algorithm, is an important engineering problem in machine learning, and this is particularly evident in today's neural networks and deep learning.

Assuming that a neural network contains 1000 parameters, each with 10 possible values, for each training/test set there are 100010 models to be examined, and thus a major problem in the tuning process is the trade-off between performance and efficiency.

In human learning, some people may have a high level of knowledge and some may have no knowledge at all. A similar classification is used in machine learning. Depending on whether the training data has label information or not, machine learning tasks can be divided into the following three categories.

1. Supervised learning: learning based on training data from known categories.

2. Unsupervised learning: learning based on unknown categories of training data.

3. Semi-supervised learning: learning using both known and unknown categories of training data.

Depending on the learning style, the better learning algorithms perform supervised learning tasks. Even AlphaGo Zero, which is claimed to be self-taught and completely independent of its dependence on Go games, is subject to Go win-loss rules and thus cannot be separated from supervised learning.

Supervised learning assumes that the training data satisfies the condition of being independently and homogeneously distributed, and learns a mapping model from the training data to the input to the output. There may be an infinite number of models reflecting this mapping, all of which together form the hypothesis space. The task of supervised learning is to find the optimal model in the hypothesis space according to a specific error criterion.

Depending on the learning method, supervised learning can be divided into two categories: generative and discriminative methods.

The generative method determines the conditional probability distribution P(Y|X) based on the joint probability distribution between input data and output data, which represents the generative relationship between input X and output Y. The discriminant method directly learns the conditional probability distribution P(Y|X) or the decision function f(X), which represents the prediction method to obtain output Y based on input X. The generative method is faster than the discriminant method, which is faster than the conditional probability distribution P(Y|X) or the decision function f(X).

In contrast, the generative method has a faster convergence speed and a wider range of applications, while the discriminant method has a higher accuracy and is simpler to use.


Today I have shared with you the basic principles and foundational concepts of machine learning, the highlights of which are as follows.

- Machine learning is the discipline in which computers construct probabilistic statistical models based on data and use the models to predict and analyze the data.
- Depending on the type of input and output, machine learning can be divided into three categories: classification problems, regression problems, and labeling problems.
- Overfitting is an unavoidable problem in machine learning and its effects can be reduced by selecting the right model.
- Supervised learning is currently the mainstream task of machine learning, including both generative and discriminative methods.

In the field of image recognition, high recognition rates are backed by a large number of finely labelled image samples, and the labelling of millions of digital images is undoubtedly labor-intensive.

