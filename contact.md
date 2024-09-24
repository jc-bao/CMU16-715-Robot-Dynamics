# Contact Dynamics

## Overview

- Why it is hard? A: contact leads to infinite acceleration, F=ma cannot be applied directly. Modelling, Simulating, Control and Learning is hard. 
- Common implementation
  - Smooth Contact Model: Mujoco, use something like smooth contact force, spring-damper model. 
    - Pros: Easy to implement, differentiable, easy to control. 
    - Cons: Not accurate, interpenetration, energy dissipation not physical. 
  - Hybrid/Event-Driven Method: detect contact with guard function with extra jump map. Common in control. 
    - Pros: not stiff, can use standard ODE solver. 
    - Cons: Scale poorly with number of contacts, handle simutaneous contacts poorly, not differentiable, hard to design general simulators. 
  - Time-stepping methods:compute the contact force at each time step to satisfy the contact constraint. Used in Gazebo, Dart, Gazebo. 
    - Pros: scales well with number of contacts, handle simultaneous contacts well, give correct physics. 
    - Cons: computationally expensive (solve optimization at each time step), not differentiable

## Discrete Mechanics wit Impacts

Constrained Optimization:

$$
\min_x f(x) \\
\text{s.t. } c(x) \ge 0
$$

KKT Condition:

$$
\text{stationary} \quad \frac{d}{dt} f(x) - \lambda^T c(x) = 0 \\
\text{complementary slackness} \quad \lambda c(x) = 0 \\
\text{prime feasibility} \quad c(x) \ge 0 \\
\text{dual feasibility} \quad \lambda \ge 0
$$

In practice, just replace f with (discrete) E-L equations. 

## Coulomb Friction and Maximum Dissipation Principle

- friction also leads to discontinuities when switching between sticking and sliding. 

Maximum Dissipation Principle: the optimization problem solve for friction (dissipation means the energy loss in the system):

$$
\min_b \dot{T} \\
\text{s.t. } b \le \mu n \\
$$

Interpolation: maximize the dissipation of the system while stay in the friction cone. 

For a single body system, the problem becomes a second order cone problem (convex optimization problem). 

$$
\min_b \dot{q}^T M b \\
\text{s.t. } b \le \mu n \\
$$

- note that when $\|b\| = 0$, the solver would fail. So we can use conic primal-dual interior point method to solve it. 
- a simple hack: use a smoothed 2-norm: $\|b\| = \sqrt{b^T b + \epsilon^2} - \epsilon$. 
- impact, friction and q can be solved at the same time. 

## Linear Complementarity Problem

- the above method is expensive. 
- most of simulators use linear approximation of friction cone. 

Euler integration: 

$$
M(q_n) \frac{q_{n+1} - q_n}{h} + C(q_n, v_n) v_n + G(q_n) = J(q_n)^T f_n + B(q_n) u \\
q_{n+1} = q_n + h v_{n+1}
$$

- Why Euler? use midpoint would make the problem non-linear. Now it is linear since q_{n+1} only apprear once. 
- Also, linearize the constraints $\phi(q_{n+1}) = \phi(q_n) + \frac{\partial \phi}{\partial q} (v h)$. 
- Lastly, linearize the friction cone: $\|b\|_1 \le \mu n$. 
  - 1-norm trick: define a new variable $d \ge 0 \in \mathbb{R}^4$ represent the positive force along (+x, -x, +y, -y). 

$$
b = \begin{bmatrix}
1 & 0 & -1 & 0 \\
0 & 1 & 0 & -1
\end{bmatrix} \\
[1 \ 1 \ 1 \ 1] d \le \mu n \\
$$

- Now the problem becomes a linear complementarity problem. 

general LCP:

$$
Ax+y = q \\
y \ge 0, x \ge 0 \\
y^T x = 0
$$

- LCP is a special KKT of QP. 
- every QP is an LCP. 
- But LCP can handle non-convex A. 
- LCP have fast solvers. 