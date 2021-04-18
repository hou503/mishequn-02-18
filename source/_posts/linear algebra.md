---
title: linear algebra
date: 2020-09-07 18:08:19
tags: Mathematics
category: Mathematics
---
Linear algebra is not only the basis of artificial intelligence, but of modern mathematics and the many disciplines that use modern mathematics as their primary method of analysis. From quantum mechanics to image processing, the use of vectors and matrices is essential. Behind vectors and matrices, the core meaning of linear algebra is to provide an abstract view of the world: everything can be abstracted as a combination of certain features and viewed in a static and dynamic way within a framework defined by pre-defined rules.

The most fundamental concept in linear algebra is the notion of a set. In mathematics, a set is defined as a collective of some particular objects aggregated together. The elements in a set will usually have certain commonalities, and thus can be represented by these commonalities. For the set {apples, oranges, pears }, the commonality is that they are all fruits; for the set {cows, horses, sheep }, the commonality is that they are all animals. Of course {apples, cows } can also form a set, but the two elements have no obvious commonality, and the usefulness of such a set in solving real-world problems is quite limited.

In linear algebra, an element consisting of a single number a is called a scalar: a scalar a can be an integer, a real number, or a complex number. If several scalars a1,a2,⋯,an form a sequence in a certain order, such elements are called vectors. Clearly, a vector can be seen as an extension of a scalar. The original number of a number is replaced by a set of numbers, thus bringing an increase in dimensionality, given the subscript that represents the index to uniquely identify the elements in the vector.

Each vector consists of a number of scalars, and if all the scalars of the vector are replaced with vectors of the same specification, the following matrix is obtained:

![](1.jpg)

The matrix also represents an increase in dimension relative to the vector, and each element in the matrix needs to be determined using two indexes (instead of one). Similarly, if each scalar element in the matrix is then replaced with a vector, what you get is a tensor. Intuitively, the tensor is the higher-order matrix.

If you think of each cube of a third-order Rubik's cube as a number, it is a 3x3x3 tensor, and the 3x3 matrix is exactly one face of the cube, which is a slice of the tensor. The tensor is a more complex and less intuitive concept than the vector or matrix.

Vectors and matrices are not only theoretical tools of analysis, but also the basic conditions under which computers work. Humans are able to perceive a continuously changing world, but computers can only process binary information with discrete values, and thus signals from the analog world must be digitized in both the definition and value domains in order to be stored and processed by computers. From this perspective, linear algebra is a tool for representing the real physical world in a virtual digital world.

In computer storage, scalars occupy zero-dimensional arrays; vectors occupy one-dimensional arrays, such as speech signals; matrices occupy two-dimensional arrays, such as grayscale images; and tensors occupy three-dimensional and higher-dimensional arrays, such as RGB images and video.

Describing vectors as mathematical objects requires a specific mathematical language, represented by the norm and the inner product. The norm is a measure of the size of a single vector, describing the properties of the vector itself, which serves to map the vector to a non-negative value. A generic definition of Lp paradigms is as follows.

![](2.jpg)

For a given vector, the L1 paradigm calculates the sum of the absolute values of all the elements of the vector, the L2 paradigm calculates the length of the vector in the usual sense, and the L∞ paradigm calculates the value of the largest element in the vector.

The parametric calculates the scale of a single vector, and the inner product calculates the relationship between two vectors. The expression for the inner product of two vectors of the same dimension is

![](3.jpg)
That is, the summation of the product of the corresponding elements. The inner product can represent the relative position between two vectors, i.e. the angle between the vectors. A special case is where the inner product is 0, i.e., Ps_27E8x,y Pe_27E9=0. In two-dimensional space, this means that the angle between two vectors is 90 degrees, i.e., perpendicular to each other. Whereas, on higher dimensional space, this relationship is called orthogonality. If two vectors are orthogonal, it means that they are linearly independent of each other and have no influence on each other.

In practical problems, the meaning of a vector is not only a combination of some numbers, but also a characteristic of some object or some behavior. The paradigms and inner products can handle the mathematical model of these representations of features and thus extract the implied relationships in the original object or the original behavior.

If there is a set whose elements are vectors of the same dimension (either finite or infinite), and which defines structured operations such as addition and multiplication, such a set is called a linear space, and a linear space which defines inner product operations is called an inner product space. In a linear space, any vector represents a point in an n-dimensional space; conversely, any point in space can be uniquely represented by a vector. The two are equivalent to each other.

A key issue in mapping points and vectors to each other in linear space is the choice of reference system. In real life, given longitude, latitude and altitude, it is possible to uniquely determine any location on the Earth, and thus the three-dimensional vectors (x, y, h) consisting of longitude, latitude and altitude correspond to a point in three-dimensional physical space.

However, in high-dimensional space, where intuition cannot be felt, the definition of a coordinate system is not so intuitive. It should be noted that artificial neural networks usually deal with tens of thousands of features, corresponding to a complex space with tens of thousands of dimensions, and the concept of orthogonal bases is needed.

In the inner product space, a set of two orthogonal vectors form the orthogonal basis of this space, assuming that the L2 paradigms of the base vectors in the orthogonal basis are of unit length 1, this set of orthogonal bases is the standard orthonormal basis. The purpose of orthogonal bases is to define the latitude and longitude of the inner product space. Once the orthonormal basis describing the inner product space is determined, the correspondence between the vectors and the points will be determined as well.

It is important to note that the orthogonal bases describing the inner product space are not unique. For two-dimensional spaces, the plane Cartesian and polar coordinate systems correspond to two different sets of orthogonal bases, and represent two practical ways of describing them.

An important feature of linear space is its ability to carry change. When the standard orthogonal bases used as reference systems are determined, a point in space can be represented as a vector. As the point moves from one location to another, the vector describing it also changes. The change of a point corresponds to a linear transformation of a vector, and the mathematical language for describing object changes or vector transformations is the matrix.

In a linear space, a change can be achieved in two ways: either by a change in the point itself or by a change in the reference system. In the first way, the way to make a point change is to multiply the matrix representing the change by the vector representing the object. But on the other hand, if we keep the point unchanged, but change the angle of observation, we will get a different result, as the saying goes, "There are different heights for different directions.

In this case, the role of the matrix is to transform the orthogonal basis. Therefore, there are different ways of interpreting the multiplication of matrices and vectors.

![](4.jpg)

This expression can be interpreted either as a vector x that undergoes the transformation described by matrix A becomes a vector y, or as an object that results in a vector x under the measure of the coordinate system A and a vector y under the measure of the standard coordinate system I (the unit matrix: the main diagonal element is 1 and the remaining elements are 0).

This means that the matrix can describe not only the changes, but also the reference system itself. To use a good analogy from the web: the expression Ax is equivalent to a declaration of the environment for the vector x, and the reference system used to measure it is A. If you want to use another reference system to measure it, you have to redeclare it. A transformation is applied to the coordinate system by multiplying the matrix representing the original coordinate system with the matrix representing the transformation.

An important pair of parameters to describe the matrix are the eigenvalue and the eigenvector. For a given matrix A, assume that its eigenvalue is λ and its eigenvector is x. The relationship between them is as follows.

![](5.jpg)

As mentioned earlier, a matrix represents a transformation of a vector, the effect of which is usually to apply both a direction change and a scale change to the original vector. However, for some special vectors, the effect of the matrix is only a change of scale and not a change of direction, i.e., only the effect of telescoping and not the effect of rotation. For a given matrix, such special vectors are the eigenvectors of the matrix, and the scale change coefficient of the eigenvector is the eigenvalue.

The dynamic significance of the matrix eigenvalues and eigenvectors is that they represent the rate and direction of change. If you think of the change represented by the matrix as a running man, then the eigenvalue of the matrix represents the speed at which he runs, and the eigenvector represents the direction in which he runs. But the matrix is not an ordinary person, it is a three-headed, six-armed Nezha, running at different speeds (eigenvalues) and in different directions (eigenvectors), and all the movements of the different bodies are superimposed on the matrix.

The process of solving the eigenvalues and eigenvectors of a given matrix is called eigenvalue decomposition, but the matrix that can be eigenvalue decomposed must be an n-dimensional matrix. Extending the eigenvalue decomposition algorithm over all matrices is the more general singular value decomposition.

Today I share with you the essential linear algebra foundations of AI, focusing on the interpretation of abstract concepts rather than concrete mathematical formulas, the key points of which are as follows.

The essence of linear algebra lies in the abstraction of concrete things into mathematical objects and the description of their static and dynamic properties.
The substance of a vector is a stationary point in an n-dimensional linear space.
A linear transformation describes a change in a vector or in a coordinate system used as a reference system and can be expressed as a matrix.
The eigenvalues and eigenvectors of a matrix describe the rate and direction of change.
Linear algebra is to artificial intelligence what addition is to higher mathematics.

