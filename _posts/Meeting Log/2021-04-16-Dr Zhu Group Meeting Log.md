---
layout: post
title: Dr. Zhu's Group Meeting Log
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

<br>

#### Abstract
> Under limited obsevability conditions, the speaker presents approaches to modeling, monitoring, and optimization of distribution systems.

<br>

#### Keywords

- limited observability  <br><br>
- modeling
  - linearized distribution model
  - alternating minimization  <br><br>
- monitoring
  - heterogeneous & dynamic data
  - sptio-temporal learning  <br><br>
- optimization
  - graph learning
  - Conditional Value-at-Risk (CVaR)  <br><br>


---

## Group Meeting Recap

### Motivation

- Accurate network model is crucial for distributed energy resource (DER) monitoring and optimization. <br><br>
- How to estimate an accurate model of the power system?<br><br>
- How to address monitoring and optimization tasks under limited observability? <br><br>

<br>
<br>

### Problem Formulation

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

- Difference between two consequtive time instance

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

- Estimate line parameter $\mathbf{\theta}$ bi-linear regression problem with <a href="#Group-LASSO">Group-LASSO regularization</a>

$$
\min_{\mathbf{\theta}, \{\tilde{\mathbf{s}}^{\mathcal{u}}_{t}\}}{\sum_{t}^{T}{\| \tilde{\mathbf{v}}_t} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{o}}_{t})\mathbf{\theta} - \mathbf{A}(\tilde{\mathbf{s}}^{\mathcal{u}}_{t})\mathbf{\theta} \|_2^2 + \lambda \| \tilde{\mathbf{s}}^{\mathcal{u}}_{t} \|_G}
\tag{3}
$$

<br>
<br>

#### Monitoring

- DER visibility by leveraging heterogenous & dynamic data.<br><br>
  - Smart meter data ($\mathbf{\Gamma}$): lacks in time resolution <br><br>
  - D-PMU data ($\mathbf{Z}$): lacks in spatial diversity <br><br>
- Active power matrix decompsition to low rank plus sparse model [2].<br><br>

$$
\begin{aligned}
\begin{bmatrix} \mathbf{P} \\ \mathbf{Q} \end{bmatrix} &= \mathbf{L} + \mathbf{DU} \\
& = \mathbf{L} + \begin{bmatrix} \mathbf{D}^{P} \\ \mathbf{D}^{Q} \end{bmatrix}
\end{aligned}
\tag{5}
$$

<br>
<br>

#### Optimization

- Optimal DER reactive power support problem

$$
\begin{aligned}
\mathbf{\hat{q}} = \min_{\mathbf{q} \in \mathcal{Q}}&{Losses(\mathbf{q})} \\
& \text{s.to } \begin{bmatrix} \mathbf{Xq} + \mathbf{y} - \mathbf{\bar{v}} \\ -\mathbf{Xq} - \mathbf{y} + \mathbf{\underline{v}} \end{bmatrix} \leq\mathbf{0}
\end{aligned}
\tag{6}
$$

- DER optimization <br><br>
  - Fast-acting DER inverter control

$$
\min_{\Phi}{Avg(\| \Phi(\mathbf{z}) -  \mathbf{\hat{q}}\|_{2}^{2})}
\tag{7}
$$

<br>
<br>

### Proposed Approach

#### Modeling

- Alternating Minimization (AM)<br><br>
  - Update $\mathbf{\theta}$ and $\{\tilde{\mathbf{s}}^{\mathcal{u}}_{t}\}$, alternatively.

<br>

#### Monitoring

- Spatio-temporal learning [1]<br><br>

$$
\begin{aligned}
\min&_{\mathbf{L}, \mathbf{D}}{\|\mathbf{L}\|_{*} + \lambda\|\mathbf{D}\|_{G}} \\
& \text{s.to } \mathbf{L} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{8}
$$

<br>

$$
\begin{aligned}
\min&_{\mathbf{v}, \mathbf{D}}{\frac{1}{2} \|\mathbf{v}\|_{2}^{2} + \lambda\|\mathbf{D}\|_{G}}\\
& \text{s.to } \mathbf{uv^{T}} + \mathbf{D}\mathbf{U} \text{ satisfies error bound for } \mathbf{\Gamma} \text{ and } \mathbf{Z}
\end{aligned}
\tag{9}
$$

<br>

#### Optimization

- Graph learning

$$
\mathbf{z}_{l+1} = \sigma(\mathbf{W}_l\mathbf{z}_l\mathbf{H}_l + \mathbf{b}_l)
\tag{10}
$$

<br>
<br>

### Experiment & Result

#### Modeling

- Experiment: Line reactance estimation  <br><br>
  - Zero injection: $\tilde{\mathbf{s}}^{\mathcal{u}}_{t} = 0$.  <br><br>
  - Projection: $\mathbf{P}(\tilde{\mathbf{v}}_t - \mathbf{A}(\tilde{\mathbf{s}}_t^o)\mathbf{\theta} - \mathbf{A}(\tilde{\mathbf{s}}_t^u)\mathbf{\theta} )$ such that $\mathbf{PA}(\tilde{\mathbf{s}}_t^u) = \mathbf{0}$. <br><br>
  - Alternating Minimization (AM) <br><br>

- Result

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/modeling_result.png" width="400" />
  <figcaption>Figure 1. Error of estimated system models (line parameters).</figcaption>
</figure>

<br>

#### Monitoring

- Experiment: DER visibility by leveraging heterogeneous & dynamic data.  <br><br>
- Result  <br><br>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/monitoring_result.png" width="600" />
  <figcaption>Figure 2. Recieving operating curves for EV event identification under 𝜅 number of D-PMU (left), and PV output estimation using aggregated power data (right).</figcaption>
</figure>

<br>

#### Optimization

- Experiment: Graph learning (w/o CVaR).  <br><br>
- Result  <br><br>
  - Under voltage issue → it needs  <a href="#CVaR">Conditional Value-at-Risk (CVaR)</a>.

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/optim_result.png" width="600" />
  <figcaption>Figure 3. Scatter plots of average voltage deviation per node of GNN and local models (left), and Histogram of voltage deviation showing under-voltage cases (right).</figcaption>
</figure>

<br>
<br>

### Conclusion

- Modeling  <br><br>
  - Estimated network line parameters under partial observability by solving a bi-linear regression problem by using AM algorithm.  <br><br>
- Monitoring  <br><br>
  - Improved DER visibility by leveraging heterogeneous & dynamic data.  <br><br>
- Optimization  <br><br>
  - Showed the need for CVaR method.  <br><br>

<br>

### Future Study

- Online implementation for modeling.  <br><br>
- Online implementation for DER visibility.  <br><br>
- CVaR-aware GNN learning for DER optimization.  <br><br>

<br>

---

## What I Benefit

<a name="Group-LASSO"></a>

### Regularization [3]

#### Lasso and Ridge Regressions

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
  <figcaption>Figure 4. Conventional Explanation to Sparsity Caused by Lasso. [3]</figcaption>
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

Usually, the variables of the minimization problem are not correlated. However, when there are certain constraints or correlations between variables, we need to group them appropriately.

The power injection as a minimization variable, covered in the presentation, is a good example. Since power is comprised of active and reactive power, the variable of <a href="#Eq. (3)">the minimization problem of Eq. (3)</a> has to be grouped as follows.

$$
\tilde{\mathbf{s}}_{t} = \begin{bmatrix} \tilde{p}_n ; \tilde{q}_n \end{bmatrix}
$$

<br>

<a name="CVaR"></a>

### Risk management

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/CVaR.png" width="700" />
  <figcaption>Figure 5. VaR and CVaR. [5]</figcaption>
</figure>
<br>

#### Value-at-Risk (VaR)

$$
\begin{aligned}
\int_{-\infty}^{VaR_{\beta}}&{f_{X}(x)}dx = 1 - \beta \\
& \text{where } f_{X}(x) \text{ is the marginal probability function of } X \\ 
& \text{and } \beta \in [0, 1] \text{ is the confidence level.}
\end{aligned}
$$

<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;or equivalently

$$
P[x \leq VaR_{\beta}] = 1 - \beta
$$

<br>

#### Conditional Value-at-Risk (CVaR) 

$$
\begin{aligned}
CVaR_{\beta} = & \frac{1}{1-\beta} \int_{-\infty}^{VaR_{\beta}}{x f_{X}(x)}dx \\
& \text{where } f_{X}(x) \text{ is the marginal probability function of } X
\end{aligned}
$$

<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;or equivalently

$$
CVaR_{\beta} = E[x | x \leq VaR_{\beta}]
$$

<br>

The "CVaR at $(1-\beta)$% level" is the expected return on $X$ in the worst $(1-\beta)$% of cases. CVaR is an alternative to VaR that is more sensitive to the shape of the tail of the loss distribution [6].


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

However, it was not easy for me to summarize the contents of today's meeting with this framework since there are three main topics in a single speech. Especially, it was hard to find the problem formulation part for monitoring. From my point of view, one of each topic is good enough for one individual presentation, and it would have been easier for me to fit the contents into this framework.

Therefore, I strongly suggest conducting our research and preparing our speech with this framework; then, it will help us draw better results in our research and deliver the main points more clearly to others.

<br>
<br>

### Resting

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-04-16-Dr Zhu Group Meeting Log/Decanting-Guide.png" width="500" />
  <figcaption>Figure . Decanting Guide [4]</figcaption>
</figure>

It is said that decanting is necessary to enjoy wine in its best quality. Likewise, our slides do want to have resting.

I think that taking some rest after writing (or making presentation slides) will introduce us to a higher level. To be more specific, it keeps us away from what we wrote and resets our minds, making us read more objectively.

For this reason, I suggest giving resting to your slides; then, it will allow you to have better results and makes you more confident in your speech.

<br>
<br>

---

## References

[1]S. Lin and H. Zhu, "Enhancing the Spatio-temporal Observability of Grid-Edge Resources in Distribution Grids", arXiv.org, 2021. [Online]. Available: https://arxiv.org/pdf/2102.07801.pdf. [Accessed: 18-Apr-2021].

[2]E. Candès, X. Li, Y. Ma and J. Wright, "Robust principal component analysis?", Journal of the ACM, vol. 58, no. 3, pp. 1-37, 2011. Available: 10.1145/1970392.1970395 [Accessed: 17-Apr-2021].

[3]L. Mao, “Group Lasso,” Lei Mao's Log Book. [Online]. Available: https://leimao.github.io/blog/Group-Lasso/. [Accessed: 17-Apr-2021].

[4]E. Golman, “The Ultimate Guide to Decanting &amp; Aerating Kosher Wine,” Kosherwine.com, 18-Dec-2020. [Online]. Available: https://www.kosherwine.com/discover/the-ultimate-guide-to-decanting-kosher-wine. [Accessed: 17-Apr-2021].

[5]"Measuring and Modifying Risks. CFA Level 1 - AnalystPrep", AnalystPrep. CFA® Exam Study Notes, 2021. [Online]. Available: https://analystprep.com/cfa-level-1-exam/portfolio-management/measuring-modifying-risks/. [Accessed: 17-Apr-2021].

[6]"Expected shortfall - Wikipedia", En.wikipedia.org, 2021. [Online]. Available: https://en.wikipedia.org/wiki/Expected_shortfall. [Accessed: 17-Apr-2021].