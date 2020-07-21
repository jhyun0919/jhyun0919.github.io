---
layout: post
title: Geometrical Meaning of Hessian Matrix
categories: [Research/Mathematics]
tags: [Math, Linear Algebra, Mathematics]
---
Starting with the definition of Hessian Matrix, this post will focus on the geometric meaning of Hessian matrix. Also, we will descuss about eigenvalues and eigenvector of Hessian, and introduce the application of the Hessian.

This post was written with reference to the following materials.
- [Donghoon Yeo's blog posting](https://angeloyeo.github.io/2020/06/17/Hessian.html)
- [Wikipedia > Hessian](https://en.wikipedia.org/wiki/Hessian_matrix)

---
# Hessian Matrix

## Definition
In mathematics, the Hessian matrix or Hessian is a square matrix of second-order partial derivatives of a scalar-valued function, or scalar field. It describes the local curvature of a function of many variables. [1]

Suppose $f : ℝ^n → ℝ$ is a function taking as input a vector $x ∈ ℝ^n$ and outputting a scalar $f(x) ∈ ℝ$. If all second partial derivatives of $f$ exist and are continuous over the domain of the function, then the Hessian matrix $H$ of $f$ is a square n×n matrix, usually defined and arranged as follows; [1]

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-21-geometrical meaning of hessian/hessian.png" width="60%" />
  <figcaption>Figure 1. Hessian matrix. [1]</figcaption>
</figure>




## Geometrical Meaning
<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-21-geometrical meaning of hessian/hessian gif 01.gif" width="90%" />
  <figcaption>Figure 2. Example of Hessian transform making the given function more convex. [2]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-21-geometrical meaning of hessian/hessian gif 02.gif" width="90%" />
  <figcaption>Figure 3. Example of Hessian transform making the given function more concave. [2]</figcaption>
</figure>



### Meaning of Hessian's Eigenvalues and Eigenvectors

The eigenvectors of the Hessian matrix represent the principal axis of transformation and the eigenvalues represent the degree of transformation.

## Applications

<figure align="center">
    <img src="https://jhyun0919.github.io/assets/img/2020-07-21-geometrical meaning of hessian/application 01.png" width="60%" />
  <figcaption>Figure 4. Hessian's applicaiton: Frangi filter for vessel detection. [2]</figcaption>
</figure>

<figure align="center">
    <img src="https://jhyun0919.github.io/assets/img/2020-07-21-geometrical meaning of hessian/application 02.png" width="60%" />
  <figcaption>Figure 5. Contour determination using Hessian's eigenvalues [3]</figcaption>
</figure>

---

## References
[1] “Hessian matrix,” Wikipedia, 24-Jun-2020. [Online]. Available: https://en.wikipedia.org/wiki/Hessian_matrix. [Accessed: 21-Jul-2020].
[2] Y. D. Yeo, “Geometrical meaning of Hessian Matrix,” 17-Jun-2020. [Online]. Available: https://angeloyeo.github.io/2020/06/17/Hessian.html. [Accessed: 21-Jul-2020].
[3] “Hessian Matrix of the image,” Stack Overflow, 01-Sep-1963. [Online]. Available: https://stackoverflow.com/questions/22378360/hessian-matrix-of-the-image. [Accessed: 21-Jul-2020].