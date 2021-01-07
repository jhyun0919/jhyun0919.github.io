---
layout: post
title: (Research Proj) Cascading Outage Simulator - Dynamic
categories: [Research/Energy]
tags: [Energy, Power, Cascading Outage, Dynamic, DAE]
---



---

# 1. CASCADING OUTAGE SIMULATOR

The vital significance of studying cascading outages has been recognized [].

However, since electrical power networks are very large and complex systems [], understanding the many mechanisms by which cascading outages propagate is challenging.

Cadcading outage simulators allow us to study a wide variety of different mechanisms of cascading outages.

<br>

# 2. IN THIS PAPER

This paper presents the design of and results from a new non-linear dynamic model of cascading failure in power system, **“Cascading Outage Simulator with Multiprocess Integration Capabilities” (COSMIC)**.

In COSMIC, ...

- **Dynamic components** are modeled using **differential equations**.

- **Power flows** are represented using **non-linear power flow equations**.

- **Discrete changes** (e.g., components failures, load shedding) are described by **a set of equations (constraints)**.

Given dynamic data for a power system and a set of exogenous disturbances that may trigger a cascade, COSMIC uses a recursive process to compute the impact of the triggering event by solving the differential-algebraic equations (DAEs).

Benefits

- providing an open platform for research and development that allows one to explicitly test the impact of the many assumptions that are necessary for dynamic cascading failure modeling. e.g. users can modify the existing system components, add new ones, and integrate advanced remedial control actions.

- the dynamic/adaptive time step and recursive islanded time horizons implemented in this simulator allow for faster computations during, near, steady-state regimes, and fine resolution during transient phases.

- can be easily integrated with High Performance Computing (HPC) clusters to run many simulations simultaneously at a much lower cost.

<br>

# 3. HYBRID SYSTEM MODELING IN COSMIC

## A. Hybrid differential-algebraic formulation

Dynamic power networks are modeled as sets of DAEs.

### A set of Differential equations

$$
\frac{d\mathbf{x}}{dt}=\mathbf{f}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t)) \tag{1}
$$

- $\mathbf{x}$ is a vector of continuous state variables that change with time according to a set of differential equations.

- (1) represent the machine dynamics.

- check appendix for more details.


### A set of Algebraic equations

$$
\mathbf{g}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t)) =0 \tag{2}
$$

- $\mathbf{y}$ is a vector fo continuous state variables that have pure algebraic relationships to other variables in the system.

- (2) encapsulate the standard ac power flow equations.

- load models are an important part of the algebraic equations. (ZIPE)

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/COSMIC_load_type.png" width="666" />
  <figcaption>Figure 1. Illustrates a dramatic impact of load models on algebraic convergence. [1]</figcaption>
</figure>

### A set of Constarints

$$
\mathbf{h}(t, \mathbf{x}(t), \mathbf{y}(t), \mathbf{z}(t))<0 \tag{3}
$$

- $\mathbf{z}$ is a vector of state variables that can only take integer states $( z_i ∈ [0, 1])$.

- constraints $\mathbf{h_i}(...)<0$ fails, an associated counter function $\mathbf{d_i}$ activates.

- During cascading failures, power systems undergo many discrete changes.

  - exogenous events (e.g., manual operations, weather)

  - endogenous events (e.g., automatic protective relay actions)

- The discrete event(s) will consequently change algebraic equations and the systems dynamic response, which may result in cascading failures, system islanding, and large blackouts.

<br>

## B. Relay modeling

- Major disturbances cause system oscillations as the system seeks a new equilibrium.

- These oscillations may naturally die out due to the interactions of system inertia, damping, and exciter and governor controls.

- In order to ensure that relays do not trip due to brief transient state changes, time-delays are added to each protective relay in COSMIC.

- We implemented in this model two types of time-delayed triggering algorithms:

  - fixed-time delay

  - [time-inverse delay](https://www.electrical4u.com/inverse-time-relay-definite-time-lag-relay/)

Five types of protective relays are modeled in COSMIC:

- over-current (OC) relays

- distance (DIST) relays

- temperature (TEMP) relays

- under-voltage load shedding (UVLS) relays

- under-frequency load shedding (UFLS)


<br>

## C. Solving the hybrid DAE

Because of its numerical stability advantages, COSMIC uses the trapezoidal rule to simultaneously integrate and solve the differential and algebraic equations.

$$
0 = \mathbf{x} + \frac{t_d-d}{2} [\mathbf{f}+\mathbf{f}(t_d, \mathbf{x}_d, \mathbf{y}_d, \mathbf{z}_d)] \tag{4}
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

- Whereas many of the common tools in the literature use a fixed time step-size, COSMIC implements a variable time step-size in order to trade-off between the diverse time-scales of the dynamics that we implement.

- TRADE-OFF

  - small

  - large

<br>

### Trapezoidal Rule

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Trapezoidal_Rule.png" width="600" />
  <figcaption>Figure .  Trapezoidal Rule []</figcaption>
</figure>

$$
\int_{a}^{b} f(x) \,dx = \frac{\Delta x}{2}[f(x_0) + 2f(x_1) +... + 2f(x_{n-1}) + f(x_n)]
$$

<br>

### Simulation Algorithm

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Algorithm.png" width="555" />
  <figcaption>Figure .  Algorithm [1]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/COSMIC_Flowchart.png" width="900" />
  <figcaption>Figure .  Algorithm Flowchart []</figcaption>
</figure>

<br>

## D. Validation

To validate COSMIC, we compared the dynamic response in COSMIC against commercial software—PowerWorld— using the classic 9-bus test case.

From a random contingency simulation, the Mean Absolute Error (MAE) between the results produced by COSMIC and PowerWorld was within 0.11%.

<br>

# 4. EXPERIMENTS AND RESULTS

Three test systems are prepared:
- 9-bus system
- 39-bus system
- 2383-bus system

<br>

## A. Polar formulation vs. Rectangular formulation in computational efficiency

### Purpose

- To compare the computational efficiency of the two formulations.

<br>

### Used test systems

- the 39-bus system
- the 2383-bus system

<br>

### Results

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/rect_polar_performance.png" width="600" />
  <figcaption>Figure . performance [1]</figcaption>
</figure>

<br>

## B. Relay event illustration

### Purpose

- To depict the functionality of how protective relays integrate with COSMIC’s time delay features.

<br>

### Used test systems

- the 9-bus system

<br>

### Results

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Relay_Events.png" width="600" />
  <figcaption>Figure . Relay Events [1]</figcaption>
</figure>

<br>

## C. Cascading outage examples

### Purpose

- To demonstrates cascading outage examples.

<br>

### Used test systems

- the 39-bus system
- the 2383-bus system

<br>

### Results

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/39Bus_Case_Cascading_Outage_Example.png" width="600" />
  <figcaption>Figure . 39-Bus Case Cascading Outage Example [1]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/2383Bus_Case_Cascading_Outage_Example.png" width="600" />
  <figcaption>Figure .  2383-Bus Case Cascading Outage Example[1]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/Branch_Outage_and_Load_Shedding.png" width="900" />
  <figcaption>Figure .  [1]</figcaption>
</figure>


<br>

## D. N-2 contingency analysis

### Purpose

- For contingency analysis

<br>

### Used test systems

- the 2383-bus system

<br>

### Results

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Demand_Loss.png" width="600" />
  <figcaption>Figure .  CCDF Demand Loss [1]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Event_Length.png" width="600" />
  <figcaption>Figure .  CCDF Event Length [1]</figcaption>
</figure>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_Number_of_Branch_Outages.png" width="600" />
  <figcaption>Figure .  CCDF Number of Branch Outages [1]</figcaption>
</figure>


<br>

## E. Comparison with a dc cascading outage simulator

### Purpose

- To compare COSMIC vs. DC Simulator.

<br>

### Used test systems

- the 2383-bus system

<br>

### Results

-  The probabilities of demand losses

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/CCDF_COSMIC_vs_DC.png" width="600" />
  <figcaption>Figure .  CCDF Demand Loss for COSMIC & DC Simulator [1]</figcaption>
</figure>


-  Path Agreement Measurement

$$
R(m_1, m_2) = \sum_{i=1}^{|C|}\frac{1}{C} \frac{|A_i \cap B_i|}{|A_i \cup B_i|} \tag{8}
$$

where models $m_1$ and $m_2$ are both subjected to the same set of exogenous contingencies $C=${$c_1,c_2,...$}. It measures the average agreement in the set of dependent events that result from each contingency in each model.

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2021-01-05-COSMIC/PAM.png" width="600" />
  <figcaption>Figure . Statical Results for the Comparison between COSMIC & DC Simulator [1]</figcaption>
</figure>


<br>

# 5. CONCLUSION

- COSMIC represents a power system as a set of hybrid discrete/continuous differential algebraic equations, simultaneously simulating protection systems and machine dynamics.

- By simulating 1200 randomly chosen N−2 contingencies for a 2383-bus test case, we found that COSMIC produces heavy-tailed blackout size distributions, which are typically found in both historical blackout data and cascading failure models [].

  - WHY???

- The relative frequency of very large events may be exaggerated in dynamic model due to numerical non-convergence (about 3% of cases).

  - WHY???

- The blackout size results show that load models can substantially impact cascade sizes—cases that used constant impedance loads showed consistently smaller blackouts, relative to constant current, power or exponential models.

- The contingency simulation results from COSMIC were compared to corresponding simulations from a dc power flow based quasi-steady-state cascading failure simulator, using a new metric. The two models largely agreed for the initial periods of cascading (for about 10 events), then diverged for later stages where dynamic phenomena drive the sequence of events.

- Together these results illustrate that detailed dynamic models of cascading failure can be useful in understanding the relative importance of various features of these models.

- The particular model used in this paper, COSMIC, is likely **too slow** for many large-scale statistical analyses, but comparing detailed models to simpler ones can be helpful in understanding the relative importance of various modeling assumptions that are necessary to understand complicated phenomena such as cascading.

<br>

---

# 6. MY RESEASRCH PROJECT

## Purpose

- Forcast or detect cascading outages in power systems.

<br>

## Approach

- Use a GNN based model to predict cascading outages.

<br>

## Experiments

### Step 1. Data Generation

- Need to introduce randomness to create large size of dataset.

- How?

<br>

### Step 2. Model GNN Model Structure

- Does the model sturcture is proper for the goal?

- What is the strength of the model? and why?

<br>

### Step 3. Train the Model & Tune the Hyper-parameters

- 🤔

<br>

---

# 7. APPENDIX

## A. Differential equations in dynamic power system

### Equation for rotor speed

$$
M \frac{dw_i}{dt} = P_{m,i} - P_{g,i} - D(w_i-1)
$$

where

- $w_i$

- $P_{m,i}$

- $P_{g,i}$

- $D$

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

---
# REFERENCES
[1] J. Song, E. Cotilla-Sanchez, G. Ghanavati and P. D. H. Hines, "Dynamic Modeling of Cascading Failure in Power Systems," in IEEE Transactions on Power Systems, vol. 31, no. 3, pp. 2085-2095, May 2016.

[2] 