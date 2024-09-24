# Lagrangian Mechanics


## Recap: Euler-Lagrange Equation

Define $L = T - V$, where $T$ is kinetic energy, $V$ is potential energy. 

The Newton's Law $M\ddot{q} - \nabla V = 0$ can be written in terms of L:

$$
\frac{d}{dt} \frac{\partial L}{\partial \dot{q}} - \frac{\partial L}{\partial q} = 0
$$

Notes:
- It is coordinate invariant. (F=ma cannot be coordinate independent)
- It is the solution of following optimization problem: (**Least Action Principle, Hamilton's Principle**) 🌟
  - Interpretation: for a conservative system, the dynamics are trade off between the kinetic energy and potential energy. (i.e. moving some energy from potential to kinetic, or kinetic to potential) The path taken by the system is the one that make the average of those two as close as possible. 

$$
\min_q J(q) = \int_0^T L(q, \dot{q}, t) dt 
$$

- This is the most general form of dynamics. 


## Manipulator Dynamics

General form for articulated rigid body:

$$
L = T - V = \frac{1}{2} \dot{q}^T M(q) \dot{q} - V(q)
$$

Manipulator Equation:

$$
M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q) = \tau
$$

Notes
- $C(q, \dot{q})$ is the Coriolis force, which comes from q-dependent $M(q)$. Sometimes people include $V(q)$ in $C(q, \dot{q})$, called that "dynamic bias".
- equivalent to E-L eqn. 
- Worst case computational complexity: $O(n^3)$, but can be linear if $M(q)$ is sparse. 

Example
Cartpole

$$
L = \frac{1}{2} m_1 \dot{x}^2 + \frac{1}{2} m_2 \dot{x}^2 + \frac{1}{2} I_1 \dot{\theta}^2 + \frac{1}{2} I_2 \dot{\theta}^2 + m_2 g x \cos \theta \\
\frac{d}{dt} \frac{\partial L}{\partial \dot{q}} - \frac{\partial L}{\partial q} = M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q) = \tau \\
M(q) = \begin{bmatrix}
m_c + m_p & m_p l_p \cos \theta \\
m_p l_p \cos \theta &  m_p l_p^2
\end{bmatrix} \\
C(q, \dot{q}) = \begin{bmatrix}
- m_p l_p \dot{\theta}^2 \sin \theta \\
0
\end{bmatrix} \\
G(q) = \begin{bmatrix}
0 \\
- m_p g l_p \sin \theta
\end{bmatrix} 
$$

## External Force

The new Lagrangian is (consider the work from external force):

$$
\partial_q \int L' dt = \partial_q \int (L + F^T q) dt = 0
$$

The new E-L eqn. is:

$$
\frac{d}{dt} \frac{\partial L'}{\partial \dot{q}} - \frac{\partial L'}{\partial q} = F
$$

Notes:
- It is called "Lagrangian-D'Alembert" principle, aka "virtual work principle". 

## Constraints and Coordinates

- Often, dynamic is simplier with more coordinates. 
- So sometimes we add some coordinates to describe the system, and some constraints on those coordinates. 

with equality constraints, the E-L eqn. is:

$$
\partial_q \int L' dt = \partial_q \int (L + \lambda^T c(q)) dt = 0 \\
\frac{d}{dt} \frac{\partial L'}{\partial \dot{q}} - \frac{\partial L'}{\partial q} - \lambda^T \frac{dc(q)}{dq} = 0
$$

where $\lambda$ is the Lagrange multiplier (constraint force) and $J(q) = \frac{dc(q)}{dq}$ is the Jacobian matrix of the constraint. 

The new manipulator equation is:

$$
M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q)  = \tau + J^T \lambda
$$

Now to get $\ddot{q}, \lambda$, we create another equation comes from the constraint (since $c(q) = 0$): 

$$
\frac{d^2 c(q)}{dt^2} = \frac{d}{dt} (\frac{dc}{dq} \dot{q}) =J\ddot{q} + \frac{d}{dq}(J \dot{q}) \dot{q} = 0 \\
$$

Now the equation is:

$$
\begin{bmatrix}
M & -J^T \\
J & 0
\end{bmatrix}
\begin{bmatrix}
\ddot{q} \\
\lambda
\end{bmatrix}
= \begin{bmatrix}
\tau - C(q, \dot{q}) - G(q) \\
-\frac{d}{dq} (J \dot{q}) \dot{q}
\end{bmatrix}
$$

Example: pendulum

$$
L = \frac{1}{2} m (x^2 + y^2) - mgy \\
c = x^2 + y^2 - l^2 = 0 
$$
