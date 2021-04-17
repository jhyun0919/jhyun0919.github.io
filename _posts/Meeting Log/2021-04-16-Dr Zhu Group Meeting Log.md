---
layout: post
title: Dr. Zhu's Group Meeting Log
categories: [Research/Meeting Log]
tags: [Group Meeting]
---

### Speech Title: Grid-Aware Machine Learning for Distribution System Modeling, Monitoring, and Optimization

---

The contents covered in this article are credited to the speaker of the meeting, Dr. Zhu, and her research group members.

<br>

## TL; DR

<br>

#### Speaker

- Shanny Lin

> Abstract or Short Brief

#### Keywords

- modeling, monitoring, optimization
- partial observability
- CVaR

<br>

---

## Group Meeting Recap

### Motivation

- 



<br>
<br>

### Problem Formulation

- How to estimate an accurate model of the power system?<br><br>
- How to address monitoring and optimization tasks under limited observability? <br><br>

<br>
<br>

### Proposed Approach

<a name="Eq. (2)"></a>

#### Modeling

- Linearized distribution flow model

$$
\begin{aligned}
\mathbf{v} & \approx \mathbf{Rp} + \mathbf{Xq} \\
& = \mathbf{M}^{-T}\mathbf{D}_{r}\mathbf{M}^{-1}\mathbf{p} + \mathbf{M}^{-T}\mathbf{D}_{x}\mathbf{M}^{-1}\mathbf{q}
\end{aligned}
\tag{1}
$$

- Bi-linear regression with <a href="#Group-LASSO">Group-LASSO regularization</a>

$$
\min_{\mathbf{\theta}, \{\tilde{\mathbf{s}}^{\mathcal{U}}_{t}\}}{\sum_{t}^{T}{\| \tilde{\mathbf{v}}_t} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{O}}_{t})\mathbf{\theta} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{U}}_{t})\mathbf{\theta} \|_2^2 + \lambda \| \tilde{\mathbf{s}}^{\mathcal{U}}_{t} \|_G}
\tag{2}
$$

- Alternating Minimization (AM)<br><br>
  - between $\mathbf{\theta}$ and $\{\tilde{\mathbf{s}}^{\mathcal{U}}_{t}\}$

<br>

#### Monitoring

- DER visibility by leveraging heterogenous & dynamic data.<br><br>
  - Smart meter data ($\mathbf{\Gamma}$)<br><br>
  - D-PMU data ($\mathbf{Z}$)<br><br>

- Spatio-temporal learning<br><br>

$$
\begin{aligned}
\begin{bmatrix} \mathbf{P} \\ \mathbf{Q} \end{bmatrix} &= \mathbf{L} + \mathbf{DU} \\
& = \mathbf{L} + \begin{bmatrix} \mathbf{D}^{P} \\ \mathbf{D}^{Q} \end{bmatrix}
\end{aligned}
\tag{3}
$$

$$
\begin{aligned}
\min&_{\mathbf{L}, \mathbf{D}}{\|\mathbf{L}\|_{*} + \lambda\|\mathbf{D}\|_{G}} \\
& \text{s.to } \mathbf{L} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{4}
$$

$$
\begin{aligned}
\min&_{\mathbf{v}, \mathbf{D}}{\frac{1}{2} \|\mathbf{v}\|_{2}^{2} + \lambda\|\mathbf{D}\|_{G}}\\
& \text{s.to } \mathbf{uv^{T}} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{5}
$$

<br>

#### Optimization

- DER optimization

$$
\begin{aligned}
\mathbf{\hat{q}} = & \min_{\mathbf{q} \in \mathcal{Q}}{Losses(\mathbf{q})} \\
& \text{s.to } \begin{bmatrix} \mathbf{Xq} + \mathbf{y} - \mathbf{\bar{v}} \\ -\mathbf{Xq} - \mathbf{y} + \mathbf{\underline{v}} \end{bmatrix} \leq\mathbf{0}
\end{aligned}
\tag{6}
$$

- Graph learning

$$
\min_{\Phi}{Avg(\| \Phi(\mathbf{z}) -  \mathbf{\hat{q}}\|_{2}^{2})}
\tag{7}
$$

$$
\mathbf{z}_{l+1} = \sigma(\mathbf{W}_l\mathbf{z}_l\mathbf{H}_l + \mathbf{b}_l)
\tag{8}
$$

- Conditional Value-at-Risk (CVaR) method.

<br>
<br>

### Experiment & Result

#### Modeling

- Line reactance estimation

<br>

#### Monitoring

- DER visibility

<br>

#### Optimization

- Graph learning vs. Local learning

<br>
<br>

### Future Study

- 

<br>

---

## What I Benefit

<a name="Group-LASSO"></a>

### Regularization

#### Lasso and Ridge Regressions []

Given a dataset ${X,y}$ where $X$ is the feature and $y$ is the label for regression, we simply model it as has a linear relationship $y = X\beta$. With regularization, we can control the degree of freedom of the model parameter ($\beta$) and able to avoid the risk of overfitting.

The two representative regularizations are LASSO (L1) and Ridge (L2), and they are defined as follows.

$$
\beta^{*} = \underset{\beta}{\mathrm{argmin}} {\| y - X\beta \|_2^2} + \lambda \| \beta \|_{1}
$$

$$
\beta^{*} = \underset{\beta}{\mathrm{argmin}} {\| y - X\beta \|_2^2} + \lambda \| \beta \|_{2}
$$

Due to the shape of their constraints boundary (norm ball shape), LASSO encourage a sparse result as a solution of the optimal problem.

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/lasso-vs-ridge.png" width="500" />
  <figcaption>Figure . Conventional Explanation to Sparsity Caused by Lasso. []</figcaption>
</figure>
<br>


#### Group LASSO

Suppose the weights in $\beta$ could be grouped, the new weight vector becomes $\beta_G = \{ \beta^{(1)}, \beta^{(2)},⋯,\beta^{(m)} \}$. Each $\beta^{(l)}$ for $1 \leq l \leq m$ represents a group of weights from $\beta$.

We further group $X$ accordingly. We denote $X(l)$ as the submatrix of $X$ with columns corresponding to the weights in $\beta^{(l)}$. The optimization problem becomes

$$
\beta^{*} = \underset{\beta}{\mathrm{argmin}} {\| y - \sum_{l=1}^{m}{X^{(l)}\beta^{(l)}} \|_2^2} + \lambda \sum_{l=1}^{m} \sqrt{p_{l}}\| \beta^{(l)} \|_{1}
$$

where $p^{(l)}$ represents the number of weights in $\beta^{(l)}$.

<br>

#### Why do we need GROUPING?

Usually, the variables of the minimization problem are not correlated. However, when there are certain constraints or correlation between variables, we need to group them appropriately.

The power injections as minimization variables, covered in the presentation , is a good example. Since power is comprised of active and reactive power, the variable of <a href="#Eq. (2)">the minimization problem of Eq. (2)</a> has to be grouped as follows.

$$
\tilde{\mathbf{s}}_{t}\ = \begin{bmatrix} \tilde{p}_n & \tilde{q}_n \end{bmatrix}
$$

<br>

### CVaR


<br>

### Throw back the Question



<br>

---

## What I Contribute

### The framework of research/study

1. Motivation  <br><br>
2. Problem Formulation  <br><br>
3. Proposed Approach  <br><br>
4. Experiment & Result  <br><br>
5. Future Study  <br><br>

The above list is the framework of research that Dr. Zhu discussed at the start of this year. Almost all research papers have been written along this process, which is also the basic step of our research.

However, it was not easy for me to summarize the contents of today's meeting with this framework since there are three main topics in a single speech. From my point of view, one of each topic is good enough for one individual presentation, and it would have been easier for me to fit the contents into this framework.

Therefore, I strongly suggest conducting our research and preparing our speech with this framework; then, it will help us draw better results in our research and deliver the main points more clearly to others.

<br>

### Resting

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/Decanting-Guide.png" width="600" />
  <figcaption>Figure . Decanting Guide []</figcaption>
</figure>

It is said that decanting is necessary to enjoy wine in it's best quality. Likewise, our slides do want to have resting.



<br>

---

## References

[] L. Mao, “Group Lasso,” Lei Mao's Log Book. [Online]. Available: https://leimao.github.io/blog/Group-Lasso/. [Accessed: 17-Apr-2021].

[] “Lasso (statistics),” Wikipedia, 14-Apr-2021. [Online]. Available: https://en.wikipedia.org/wiki/Lasso_(statistics). [Accessed: 17-Apr-2021].

[] P. P. H. Winston, “How to Speak,” MIT OpenCourseWare. [Online]. Available: https://ocw.mit.edu/resources/res-tll-005-how-to-speak-january-iap-2018/. [Accessed: 17-Apr-2021].

[] E. Golman, “The Ultimate Guide to Decanting &amp; Aerating Kosher Wine,” Kosherwine.com, 18-Dec-2020. [Online]. Available: https://www.kosherwine.com/discover/the-ultimate-guide-to-decanting-kosher-wine. [Accessed: 17-Apr-2021].

[] 