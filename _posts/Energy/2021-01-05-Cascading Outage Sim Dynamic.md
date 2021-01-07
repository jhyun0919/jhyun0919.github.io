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

# 2. IN THIS PAPER... COSMIC

This paper presents the design of and results from a new non-linear dynamic model of cascading failure in power system, “Cascading Outage Simulator with Multiprocess Integration Capabilities” (COSMIC).

In COSMIC, ...

- Dynamic components are modeled using differential equations.
- Power flows are represented using non-linear power flow equations.
- Discrete changes (e.g., components failures, load shedding) are described by a set of equations.

Given dynamic data for a power system and a set of exogenous disturbances that may trigger a cascade, COSMIC uses a recursive process to compute the impact of the triggering event by solving the differential-algebraic equations (DAEs).

Benefits

- providing an open platform for research and development that allows one to explicitly test the impact of the many assumptions that are necessary for dynamic cascading failure modeling.
  - e.g. users can modify the existing system components, add new ones, and integrate advanced remedial control actions.
- the dynamic/adaptive time step and recursive islanded time horizons implemented in this simulator allow for faster computations during, near, steady-state regimes, and fine resolution during transient phases.
- can be easily integrated with High Performance Computing (HPC) clusters to run many simulations simultaneously at a much lower cost.

<br>

# 3. HYBRID SYSTEM MODELING IN COSMIC

## A. Hybrid differential-algebraic formulation

Dynamic power networks are typically modeled as sets of DAEs.

### A set of differential equations

$$
\frac{d\bold{x}}{dt}=\bold{f}(t, \bold{x}(t), \bold{y}(t), \bold{z}(t)) \tag{1}
$$

- $\bold{x}$ is a vector of continuous state variables that change with time according to a set of differential equations.
- (1) represent the machine dynamics.
- check appendix for more details.


### A set of algebraic equations

$$
\bold{g}(t, \bold{x}(t), \bold{y}(t), \bold{z}(t)) =0 \tag{2}
$$

- $\bold{y}$ is a vector fo continuous state variables that have pure algebraic relationships to other variables in the system.
- (2) encapsulate the standard ac power flow equations.
- load models are an important part of the algebraic equations. (ZIPE)


### A set of constarints

$$
\bold{h}(t, \bold{x}(t), \bold{y}(t), \bold{z}(t))<0 \tag{3}
$$

- $\bold{z}$ is a vector of state variables that can only take integer states $( z_i ∈ [0, 1])$.
- constraints $\bold{h_i}(...)<0$ fails, an associated counter function \bold{d_i} activates.

<br>

## B. Relay modeling


<br>

## C. Solving the hybrid DAE


<br>

## D. Validation


<br>

# 4. EXPERIMENTS AND RESULTS

## A. Polar formulation vs. Rectangular formulation in computational efficiency


<br>

## B. Relay event illustration


<br>

## C. Cascading outage examples using the 39-bus and the 2383-bus power systems


<br>

## D. N-2 contingency analysis using the 2484-bus case


<br>

## E. Comparison with a dc cascading outage simulator


<br>

# 5. CONCLUSION


<br>

# 6. FOR PERSONAL RESEASRCH PROJ


<br>

# 7. REFERENCES
[1] J. Song, E. Cotilla-Sanchez, G. Ghanavati and P. D. H. Hines, "Dynamic Modeling of Cascading Failure in Power Systems," in IEEE Transactions on Power Systems, vol. 31, no. 3, pp. 2085-2095, May 2016.

[2] 