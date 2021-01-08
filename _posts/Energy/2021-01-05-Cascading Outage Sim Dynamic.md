---
layout: post
title: (Research Proj) Cascading Outage Simulator - Dynamic
categories: [Research/Energy]
tags: [Energy, Power, Cascading Outage, Dynamic, DAE, Research Proj]
---

This article is about a review of the paper [1], and is the first part of a personal research project to apply Graph Neural Network (GNN) to the field of cascading outage prediction or detection.

The primary purpose is to create large-scale datasets for training GNN-based models through the cascading outage simulator presented in this paper.

- [Git Repository](https://github.com/jhyun0919/GNN-and-Power-Systems/tree/master/Cascading%20Outage)

- [Research Notes in GoodNotes]()

- [Paper Review in GoodNotes](https://goodnotes.com/shares/#aHR0cHM6Ly93d3cuaWNsb3VkLmNvbS9zaGFyZS8wMkdiT3EyOTc0TERRd0NUdWtmeVF3MzBnI0R5bmFtaWNfTW9kZWxpbmdfb2ZfQ2FzY2FkaW5nX0ZhaWx1cmVfaW5fUG93ZXJfU3lzdGVtcw== )

---

# 1. CASCADING OUTAGE SIMULATOR

<br>

## WHAT & WHY

The importance of studying cascading outages has been recognized []. However, since electrical power networks are very large and complex systems [], understanding the many mechanisms by which cascading outages propagate is challenging [1].

Cadcading outage simulators are designed to set and reproduce scenarios that can occur in the electric grid, and it allow us to study a wide variety of different mechanisms of cascading outages.

<br>

---

## IN THE PAPER

This paper presents the design of and results from a new non-linear dynamic model of cascading failure in power system,  called **“Cascading Outage Simulator with Multiprocess Integration Capabilities” (COSMIC)**.

In COSMIC [1], ...

- **Dynamic components** are modeled using **differential equations**.

- **Power flows** are represented using **non-linear power flow equations**.

- **Discrete changes** (e.g., components failures, load shedding) are described by **a set of equations (constraints)**.

COSMIC has the following benefits [1].

- It provids an **open platform** for research and development.

- The **dynamic/adaptive time step** and **recursive islanded time horizons** implemented in this simulator which allows for faster computations during, near, steady-state regimes, and fine resolution during transient phases.

- It can be easily integrated with High Performance Computing (HPC) clusters to **run many simulations simultaneously** at a much lower cost.

<br>

---

# 2. HYBRID SYSTEM MODELING IN COSMIC

<br>

## A. Hybrid differential-algebraic formulation

Dynamic power networks are modeled as sets of DAEs. Each DAE is composed of three parts:

  ```
  i. A set of Differential equations
  ii. A set of Algebraic equations
  iii. A set of Constarints
  ```

<br>

### i. A set of Differential equations

$$
\frac{d\mathbf{x}}{dt}=\mathbf{f}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t)) \tag{1}
$$

- $\mathbf{x}$ is a vector of **continuous state variables** that change with time according to a set of **differential equations**.

- (1) represent the **machine dynamics** (*APPENDIX - A*).

<br>

### ii. A set of Algebraic equations

$$
\mathbf{g}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t)) =0 \tag{2}
$$

- $\mathbf{y}$ is a vector fo **continuous state variables** that have pure **algebraic relationships** to other variables in the system.

- (2) encapsulate the standard **ac power flow equations** (*APPENDIX - B*).

- (2) is largely dependent on the load models (Figure 1. ZIPE models).

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/COSMIC_load_type.png" width="666" />
  <figcaption>Figure 1. Illustrates a dramatic impact of load models on algebraic convergence. [1]</figcaption>
</figure>

<br>

### iii. A set of Constarints

$$
\mathbf{h}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t))<0 \tag{3}
$$

- $\mathbf{z}$ is a vector of **state variables** (relay status) that can only take **integer states** $( z_i ∈ [0, 1])$.

- (3) represent the **constraints**.

- If a constraint $\mathbf{h_i}(...)<0$ fails (outage occurs), an associated **counter function** $\mathbf{d_i}$ (relay) activates.

<br>

### cf. What happens during cascading failures?

During cascading failures, power systems many undergo **discrete changes**. The discrete event(s) will consequently **change the systems dynamic response and algebraic equations**, which may result in cascading failures, system islanding, and large blackouts.

  <!-- - exogenous events (e.g., manual operations, weather) -->

  <!-- - endogenous events (e.g., automatic protective relay actions) -->

<br>

---

## B. Relay modeling

<!-- Major disturbances cause system oscillations as the system seeks a new equilibrium. These oscillations may naturally die out due to the interactions of system inertia, damping, and exciter and governor controls. In order to ensure that relays do not trip due to brief transient state changes, time-delays are added to each protective relay in COSMIC. -->

Major disturbances cause system oscillations, and these oscillations may naturally die out as the system adjusts to a new equilibrium. In order to ensure that relays do not trip due to brief transient state changes, time-delays are added to each protective relay in COSMIC.

Five types of protective relays are modeled in COSMIC:

- Over-current (OC) relays

- Distance (DIST) relays

- Temperature (TEMP) relays

- Under-voltage load shedding (UVLS) relays

- Under-frequency load shedding (UFLS)

<br>

---

## C. Solving the hybrid DAE

COSMIC used the following two strategies to solve the hybrid DAE.

- COSMIC uses the **trapezoidal rule** to simultaneously integrate and solve the differential and algebraic equations.
  <!-- - numerical stability -->

- COSMIC implements a **variable time-step size** in order to trade-off between the diverse time-scales of the dynamics.

  - During transition periods: small step size → **fine resolution**.

  - During steady-state periods: large step size → **faster computation**.

<br>

---

### Trapezoidal Rule

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Trapezoidal_Rule.png" width="600" />
  <figcaption>Figure 2.  Trapezoidal Rule []</figcaption>
</figure>

$$
\int_{a}^{b} f(x) \,dx = \frac{\Delta x}{2}[f(x_0) + 2f(x_1) +... + 2f(x_{n-1}) + f(x_n)]
$$

<!-- <p style="text-align: center;"> ↓ </p>


$$
\mathbf{x}_+ = \mathbf{x} + \frac{\Delta t}{2} [\mathbf{f}(t)+\mathbf{f}(t_+, \mathbf{x}_+, \mathbf{y}_+, \mathbf{z})]
$$

$$
0 = \mathbf{g}(t_+, \mathbf{x}_+, \mathbf{y}_+, \mathbf{z})
$$

- $\mathbf{x}_+=\mathbf{x}(t+\Delta t)$
- $\mathbf{y}_+=\mathbf{y}(t+\Delta t)$ -->

<br>

---

### DAE during discrete event

$$
0 = \mathbf{x} + \frac{t_d-t}{2} [\mathbf{f}(t)+\mathbf{f}(t_d, \mathbf{x}_d, \mathbf{y}_d, \mathbf{z}_d)] \tag{4}
$$

$$
0=\mathbf{g}(t_d, \mathbf{x}_+, \mathbf{y}_+, \mathbf{z}) \tag{5}
$$

$$
0>\mathbf{h}(t_d, \mathbf{x}_+, \mathbf{y}_+) \tag{6}
$$

$$
0=\mathbf{d}(t_d, \mathbf{x}_+, \mathbf{y}_+) \tag{7}
$$

where

- $t$ is the previous time point.

- $t_d$ is the point a discrete event occurs.

Because of the adaptive time step size, COSMIC retains $t_d$ from $t_d = t + \Delta t_d$, in which $\Delta t_d$ is found by **linear interpolation** of two time steps.

<br>

---

### Time-Domain Simulation Algorithm

The description of Time-Domain Simulation Algorithm implemented in COSMIC and a corresponding flowchart are as follows.

The feature of this algorithm that we need to focus is that when network separation occurs, each subnetwork is computed **independently** in a **recursive** manner.

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Algorithm.png" width="555" />
  <figcaption>Figure 3. Time-Domain Simulation Algorithm [1]</figcaption>
</figure>

<br>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/COSMIC_Flowchart.png" width="900" />
  <figcaption>Figure 4. Flowchart of Time-Domain Simulation Algorithm</figcaption>
</figure>

<br>

---

# 3. EXPERIMENTS AND RESULTS

In this paper, the performance and characteristics of COSMIC were explored through several experiments as follows.

<br>

---

## A. Polar formulation vs. Rectangular formulation in computational efficiency

### Purpose

- To compare the computational efficiency of the polar and rectangular formulations of the model.
  - WHY???

<br>

### Used test systems

- 39-bus system

- 2383-bus system

<br>

### Results

- From Figure 5. Table I, in 39-bus case, the rectangular formulation required fewer linear solves for different demand losses.

- From Figure 5. Table II, in 2383-bus case, there was no significant improvement for the rectangular formulation over the polar formulation, and the number of linear solves that resulted from both forms were almost identical.
  - WHY DENSITY OF JACOBIAN???

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/rect_polar_performance.png" width="600" />
  <figcaption>Figure 5. Performance comparison between Rect & Polar.  [1]</figcaption>
</figure>

<br>

---

## B. Relay event illustration

### Purpose

- To illustrate the different relay functions implemented in COSMIC and their time delay algorithms.

<br>

### Used test systems

- 9-bus system

<br>

### Results

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Relay_Events.png" width="600" />
  <figcaption>Figure 6. Bus voltage magnitudes when the branch from bus 6 to bus 9 in the 9-bus system is tripped.  [1]</figcaption>
</figure>

- A single-line outage was occured at $t=10$ seconds, and  DIST relay timer activate.

- $t_{preset-delay}=0.5$

- $P_1 (t=10.5)$: $t_{delay}$ ran out, and another line outage was occured.

- $P_2$: the magenta voltage trace violated the limit, and UVLS relay timer activate.

- $P_3$: UVLS relay took action and shed 25% of the initial load at the bus.

<br>

---

## C. Cascading outage examples

### Purpose

- To demonstrates how COSMIC processes cascading events such as line branch outages, load shedding, and is-landing.

<br>

### Used test systems

- 39-bus system

- 2383-bus system

<br>

### Results

#### 39-Bus Case

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/39Bus_Case_Cascading_Outage_Example.png" width="600" />
  <figcaption>Figure 7. 39-Bus Case Cascading Outage Example [1]</figcaption>
</figure>

- At  $t=3.00$ sec, the system suffered a strong dynamic oscillation after the initial two exogenous events (branches 2–25 and 5–6).

- At  $t=54.06$ sec, the first OC relay at branch 4–5 triggered.

- At  $t=55.06$ and $t=55.07$ sec, load shedding at two buses (Bus 7 and Bus 8) occurred.

- At $t=55.28$ sec, another two branches (10–13 and 13–14) shut down after OC relay trips. These events separated the system into two islands.

- At $t=55.78$ sec, two branches (3–4 and 17–18) were taken off the grid and this resulted in another island. The system eventually ended up with three isolated networks.

<br>

#### 2383-Bus Case

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/2383Bus_Case_Cascading_Outage_Example.png" width="600" />
  <figcaption>Figure 8.  2383-Bus Case Cascading Outage Example [1]</figcaption>
</figure>

- Number 0 with black highlights denotes the two initial events (N-2 contingency).

- Other sequential numbers indicate the rest of the branch outages.

- In this example, 24 branches are off-line and cause a small island (within the dashed circle) in the end.

- The dots with additional red squares indicate buses where load shedding happens.

<br>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Branch_Outage_and_Load_Shedding.png" width="900" />
  <figcaption>Figure 9. Branch outage events & Load-shedding events listed in Figure 8 [1]</figcaption>
</figure>

<br>

The top panel in Figure 9 shows the timeline of all branch outage events for the 2383-bus cascading scenario, and the lower panel zooms in the load-shedding events.

What we found here are ...

- In the early phase of this cascading outages, the occurrence of the components failed relatively slowly, but it speeds up as the number of failures increased (check top panel).

- When the system condition was substantially compromised, fast collapse occurs and the majority of the branch undergo outages as well as the load shedding events (check lower panel).

<br>

---

## D. N-2 contingency analysis

### Purpose

- To studies the impact of different load modeling assumptions on cascading failure sizes.

<br>

### Used test systems

- 2383-bus system

<br>

### Results

#### Demand Loss

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Demand_Loss.png" width="600" />
  <figcaption>Figure 10.  CCDF Demand Loss [1]</figcaption>
</figure>

<br>

#### Event Length

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Event_Length.png" width="600" />
  <figcaption>Figure 11.  CCDF Event Length [1]</figcaption>
</figure>

<br>

#### Number of Branch Outages

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Number_of_Branch_Outages.png" width="600" />
  <figcaption>Figure 12.  CCDF Number of Branch Outages [1]</figcaption>
</figure>

<br>

---

## E. Comparison with a dc cascading outage simulator

### DC model

- Pros
  - It is numerically stable, making it possible to produce results that can be statistically similar to data from real power systems [].

- Cons
  - It includes numerous simplifications that are substantially different from the “real” system.

### Dynamic model

- Pros
  - It includes many mechanisms of cascading that cannot be represented in the dc model.

- Cons
  - ???


<br>

---

### Purpose

- To compare results from COSMIC with results from a dc-power flow based model of cascading failure in order to understand similarities and differences between these two different modeling approaches.

<br>

### Used test systems

- 2383-bus system

<br>

### Results

#### The probabilities of demand losses

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_COSMIC_vs_DC.png" width="600" />
  <figcaption>Figure 13.  CCDF Demand Loss for COSMIC & DC Simulator [1]</figcaption>
</figure>

- the probability of demand losses in the dc simulator is lower than that of COSMIC for the same amount of demand losses. In particular, the largest demand loss in dc simulator is much smaller than in COSMIC (2639 MW versus 24602 MW, with probabilities 0.08% versus 2.5%). This large difference between them is not surprising because the dc model is much more stable and does not run into problems of nu- merical nonconvergence. Also, the protection algorithms differ somewhat between the two models. In addition, some of the contingencies do produce large blackouts in the dc simulator, which causes the fat tail that can be seen in Fig. 9. Numerical failures in solving the DAE system greatly con- tributed to the larger blackout sizes observed in COSMIC, be- cause COSMIC assumes that the network or sub-network in which the numerical failure occurred experienced a complete blackout. This illustrates a tradeoff that comes with using de- tailed nonlinear dynamic models: while the component models are more accurate, the many assumptions that are needed sub- stantially impact the outcomes, potentially in ways that are not fully accurate.
It is possible that this result may be affected to some extent by the size of the sample set. The 1200 randomly selected con- tingency pairs represent 0.0278% of the total branch outage pairs. Extensive investigation of how different sampling approaches might impact the observed statistics remains for fu- ture work.

<br>

#### Path Agreement Measurement

$$
R(m_1, m_2) = \sum_{i=1}^{|C|}\frac{1}{C} \frac{|A_i \cap B_i|}{|A_i \cup B_i|} \tag{8}
$$

where

- models $m_1$ and $m_2$ are both subjected to the same set of exogenous contingencies $C=${$c_1,c_2,...$}.

- It measures the average agreement in the set of dependent events that result from each contingency in each model.

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/PAM.png" width="600" />
  <figcaption>Figure 14. Statical Results for the Comparison between COSMIC & DC Simulator [1]</figcaption>
</figure>

- Table V shows that the average between the two models for the whole set of sequences is 0.1948, which is relatively low. This indicates that there are substantial differ- ences between cascade paths in the two models. Part of the reason is that the dc model tends to produce longer cascades and consequently increase the denominator in (9). In order to control this, we computed only for the first ten branch outage events. The average R increased to 0.3487, and some of the cascading paths showed a perfect match ( ). This suggests that the cascading paths resulting from the two models tend to agree during the early stages of cascading, when nonlinear dynamics are less pronounced, but disagree during later stages.

<br>

---

# 4. CONCLUSION

Through the above experiments, the following conclusions are drawn.

- COSMIC represents a power system as a set of **hybrid discrete/continuous differential algebraic equations**, simultaneously simulating protection systems and machine dynamics.

- From the N−2 contingency analysis, we found that COSMIC produces **heavy-tailed** blackout size distributions, which are typically found in both historical blackout data and cascading failure models [].

  - WHY???

- The relative frequency of very large events may be exaggerated in dynamic model due to numerical non-convergence (about 3% of cases).

  - WHY???

- The blackout size results show that load models can substantially impact cascade sizes.

- The contingency simulation results from COSMIC were compared to corresponding simulations from a dc power flow based quasi-steady-state cascading failure simulator, using a new metric.

- The two models largely agreed for the initial periods of cascading (for about 10 events), then diverged for later stages where dynamic phenomena drive the sequence of events.

- Detailed dynamic models of cascading failure can be useful in understanding the relative importance of various features of these models.

<br>

---

# 5. MY RESEASRCH PROJECT

## Purpose

- Forcast or detect (in real-time) cascading outages in power systems.

<br>

## Approach

- Use a GNN based model to predict or detect cascading outages.

<br>

## Experiments

### Step 1. Data Generation

- Need to introduce randomness to create large size of dataset.

- How?

- [generate_dataset.ipynb]()

<br>

### Step 2. Model GNN Model Structure

- Does the model sturcture is proper for the goal?

- What is the strength of the model? and why?

<br>

### Step 3. Train the Model & Tune the Hyper-parameters

- 🤔

<br>

---

# 6. APPENDIX

## A. Differential equations in dynamic power system [1]

### Equation for rotor speed

$$
M \frac{dw_i}{dt} = P_{m,i} - P_{g,i} - D(w_i-1)
$$

where

- $w_i$

- $P_{m,i}$

- $P_{g,i}$

- $D$

<br>

### Equation for rotor angle

$$
\frac{d\delta_i(t)}{dt} = 2 \pi f_0 (w_i-1)
$$

where

- $delta_i(t)$

- $f_0$

- $w_i$

<br>

etc.

<br>

## B. AC power flow equation []

Relation between following three compenets.

- **real/reactive power** injected into each bus ($P_i, Q_i; i=1, 2, ..., n$)

- **bus voltage magnitude** of each bus ($V_i; i=1, 2, ..., n$)

- **phase angle** of each bus ($\theta_i; i=1, 2, ..., n$).

<!-- $$
S_i = P_i + jQ_i\\
= \bar{V_i} \bar{I_i}^*\\
= \bar{V_i} ( \sum_{k=1}^{n} \bar{Y_{ik}} \bar{V_k} )^*\\
$$ -->

<br>

$$
P_i = V_i\sum_{k=1}^{n}\{ G_{ik} cos(\theta_i-\theta_k) +B_{ik}sin(\theta_i-\theta_k) \}
$$

$$
Q_i = V_i\sum_{k=1}^{n}\{ G_{ik} sin(\theta_i-\theta_k) +B_{ik}cos(\theta_i-\theta_k) \}
$$

where
- $n$ is the number of buses of the system.

- $Y_{ik} = G_{ik} + j B_{ik}$ is (i, k)-th entry of Y-bus matrix.

<br>

---
# REFERENCES
[1] J. Song, E. Cotilla-Sanchez, G. Ghanavati and P. D. H. Hines, "Dynamic Modeling of Cascading Failure in Power Systems," in IEEE Transactions on Power Systems, vol. 31, no. 3, pp. 2085-2095, May 2016.

[2] 

[] “Trapezoidal Rule,” Math24, 30-Apr-2020. [Online]. Available: https://www.math24.net/trapezoidal-rule/. [Accessed: 07-Jan-2021].

[] 