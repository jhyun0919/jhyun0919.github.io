---
layout: post
title: (eiting👷🏻‍♂️) Dr. Zhu's Group Meeting Log
categories: [Research/Meeting Log]
tags: [Group Meeting]
---

### **Speech Title**: Grid-Aware Machine Learning for Distribution System Modeling, Monitoring, and Optimization

---

The contents covered in this article are credited to the speaker of the meeting, Dr. Zhu, and her research group members.

<br>

## TL; DR

<br>

#### Speaker

- Shanny Lin

> Abstract or Short Brief

#### Keywords

- modeling
  - partial observability
  - linearized distribution model
  - alternating minimization  <br><br>
- monitoring
  - sptio-temporal learning  <br><br>
- optimization
  - graph learning, CVaR  <br><br>

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

#### Modeling

- Linearized distribution flow model

$$
\begin{aligned}
\mathbf{v} & \approx \mathbf{Rp} + \mathbf{Xq} \\
& = \mathbf{M}^{-T}\mathbf{D}_{r}\mathbf{M}^{-1}\mathbf{p} + \mathbf{M}^{-T}\mathbf{D}_{x}\mathbf{M}^{-1}\mathbf{q}
\end{aligned}
\tag{1}
$$

<br>

$$
\begin{aligned}
\tilde{\mathbf{v}_t} & := \mathbf{v}_t - \mathbf{v}_{t-1} \\
& = \mathbf{R}(\mathbf{r})\tilde{\mathbf{p}_t} + \mathbf{X}(\mathbf{x})\tilde{\mathbf{q}_t} \\
& = \mathbf{A}(\begin{bmatrix} \tilde{p}_n ; \tilde{q}_n \end{bmatrix})\mathbf{\theta} \\
& = \mathbf{A}(\tilde{\mathbf{s}}_{t})\mathbf{\theta}
\end{aligned}
\tag{2}
$$

<a name="Eq. (3)"></a>

- Bi-linear regression with <a href="#Group-LASSO">Group-LASSO regularization</a>

$$
\min_{\mathbf{\theta}, \{\tilde{\mathbf{s}}^{\mathcal{u}}_{t}\}}{\sum_{t}^{T}{\| \tilde{\mathbf{v}}_t} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{o}}_{t})\mathbf{\theta} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{u}}_{t})\mathbf{\theta} \|_2^2 + \lambda \| \tilde{\mathbf{s}}^{\mathcal{u}}_{t} \|_G}
\tag{3}
$$

- Alternating Minimization (AM)<br><br>
  - between $\mathbf{\theta}$ and $\{\tilde{\mathbf{s}}^{\mathcal{u}}_{t}\}$

<br>

#### Monitoring

- DER visibility by leveraging heterogenous & dynamic data.<br><br>
  - Smart meter data ($\mathbf{\Gamma}$)<br><br>
  - D-PMU data ($\mathbf{Z}$)<br><br>

- Spatio-temporal learning [] <br><br>

$$
\begin{aligned}
\begin{bmatrix} \mathbf{P} \\ \mathbf{Q} \end{bmatrix} &= \mathbf{L} + \mathbf{DU} \\
& = \mathbf{L} + \begin{bmatrix} \mathbf{D}^{P} \\ \mathbf{D}^{Q} \end{bmatrix}
\end{aligned}
\tag{4}
$$

<br>

$$
\begin{aligned}
\min&_{\mathbf{L}, \mathbf{D}}{\|\mathbf{L}\|_{*} + \lambda\|\mathbf{D}\|_{G}} \\
& \text{s.to } \mathbf{L} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{5}
$$

<br>

$$
\begin{aligned}
\min&_{\mathbf{v}, \mathbf{D}}{\frac{1}{2} \|\mathbf{v}\|_{2}^{2} + \lambda\|\mathbf{D}\|_{G}}\\
& \text{s.to } \mathbf{uv^{T}} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{6}
$$

<br>

#### Optimization

- DER optimization

$$
\begin{aligned}
\mathbf{\hat{q}} = & \min_{\mathbf{q} \in \mathcal{Q}}{Losses(\mathbf{q})} \\
& \text{s.to } \begin{bmatrix} \mathbf{Xq} + \mathbf{y} - \mathbf{\bar{v}} \\ -\mathbf{Xq} - \mathbf{y} + \mathbf{\underline{v}} \end{bmatrix} \leq\mathbf{0}
\end{aligned}
\tag{7}
$$

- Graph learning

$$
\min_{\Phi}{Avg(\| \Phi(\mathbf{z}) -  \mathbf{\hat{q}}\|_{2}^{2})}
\tag{8}
$$

$$
\mathbf{z}_{l+1} = \sigma(\mathbf{W}_l\mathbf{z}_l\mathbf{H}_l + \mathbf{b}_l)
\tag{9}
$$

- <a href="#CVaR">Conditional Value-at-Risk (CVaR)</a> method.

<br>
<br>

### Experiment & Result

#### Modeling

- Experiment: Line reactance estimation  <br><br>
  - Zero injection: $\tilde{\mathbf{s}}^{\mathcal{u}}_{t} = 0$.  <br><br>
  - Projection: $\mathbf{P}(\tilde{\mathbf{v}}_t - \mathbf{A}(\tilde{\mathbf{s}}_t^o)\mathbf{\theta} - \mathbf{A}(\tilde{\mathbf{s}}_t^u)\mathbf{\theta} )$ such that $\mathbf{PA}(\tilde{\mathbf{s}}_t^u) = \mathbf{0}$. <br><br>
  - AM <br><br>

- Result

<br>

#### Monitoring

- Experiment: DER visibility by leveraging heterogeneous & dynamic data.  <br><br>
- Result  <br><br>
  - Recieving operating curves for EV event identification under $\kappa$ number of D-PMU. <br><br>
  - PV output estimation using aggregated power data <br><br>

<br>

#### Optimization

- Experiment: Graph learning (w/o CVaR).  <br><br>
- Result  <br><br>
  - GNN reduces average voltage deviation per node.  <br><br>
  - Histogram of voltage deviation shows *under-voltage cases*.  <br><br>
    - Need CVaR!

<br>
<br>

### Conclusion

- Modeling  <br><br>
- Monitoring  <br><br>
- Optimization  <br><br>

<br>

### Future Study

- Online implementation for modeling.  <br><br>
- Online implementation for DER visibility.  <br><br>
- CVaR-aware GNN learning for DER optimization.  <br><br>

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

where $p_{l}$ represents the number of weights in $\beta^{(l)}$.

<br>

#### Why do we need GROUPING?

Usually, the variables of the minimization problem are not correlated. However, when there are certain constraints or correlation between variables, we need to group them appropriately.

The power injections as minimization variables, covered in the presentation , is a good example. Since power is comprised of active and reactive power, the variable of <a href="#Eq. (3)">the minimization problem of Eq. (3)</a> has to be grouped as follows.

$$
\tilde{\mathbf{s}}_{t} = \begin{bmatrix} \tilde{p}_n ; \tilde{q}_n \end{bmatrix}
$$

<br>

<a name="CVaR"></a>
### CVaR



<br>

---

## What I Contribute

### The framework of research/study
  ```
  1. Motivation
  2. Problem Formulation
  3. Proposed Approach
  4. Experiment & Result
  5. Future Study
  ```

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

[1]S. Lin and H. Zhu, "Enhancing the Spatio-temporal Observability of Grid-Edge Resources in Distribution Grids", arXiv.org, 2021. [Online]. Available: https://arxiv.org/pdf/2102.07801.pdf. [Accessed: 18-Apr-2021].

[]L. Mao, “Group Lasso,” Lei Mao's Log Book. [Online]. Available: https://leimao.github.io/blog/Group-Lasso/. [Accessed: 17-Apr-2021].

[]“Lasso (statistics),” Wikipedia, 14-Apr-2021. [Online]. Available: https://en.wikipedia.org/wiki/Lasso_(statistics). [Accessed: 17-Apr-2021].

[]P. P. H. Winston, “How to Speak,” MIT OpenCourseWare. [Online]. Available: https://ocw.mit.edu/resources/res-tll-005-how-to-speak-january-iap-2018/. [Accessed: 17-Apr-2021].

[]E. Golman, “The Ultimate Guide to Decanting &amp; Aerating Kosher Wine,” Kosherwine.com, 18-Dec-2020. [Online]. Available: https://www.kosherwine.com/discover/the-ultimate-guide-to-decanting-kosher-wine. [Accessed: 17-Apr-2021].

[] 