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

where $\lambda$ is the Lagrange multiplier (**constraint force**) and $J(q) = \frac{dc(q)}{dq}$ is the Jacobian matrix of the constraint. 

The new manipulator equation is:

$$
M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q)  = \tau + J^T \lambda
$$

Now to get $\ddot{q}, \lambda$, we create another equation comes from the constraint (since $c(q) = 0$): 

$$
\frac{d^2 c(q)}{dt^2} = \frac{d}{dt} (\frac{dc}{dq} \dot{q}) =J\ddot{q} + \frac{d}{dq}(J \dot{q}) \dot{q} = 0 \\
$$

Now the equation is: (KKT condition)

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

- you can also solve the $\lambda$ first, then plug it into the manipulator equation to get $\ddot{q}$. But the conditional number would be higher, leading to numerical instability. 

Example: pendulum

$$
L = \frac{1}{2} m (x^2 + y^2) - mgy \\
c = x^2 + y^2 - l^2 = 0 
$$

## Differential Algebraic Equations

Try to simulate the system without drifting due to the discretization (i.e. make sure the constraint is always satisfied): 

$$
\dot{x} = f(x, u, t) \\
c(x, u, t) = 0
$$

**Baumgarte Stabilization**: 
Idea is try to convert the DAEs into ODEs by adding a damping/feedback term to the constraints. 

$$
M(q) \ddot{q} + C(q, \dot{q}) \dot{q} + G(q)  = \tau + J^T (\lambda - \alpha c(q, \dot{q}, t) - \beta \dot{c}(q, \dot{q}, t))
$$

Notes
- now the constraint is Lyapunov stable
- but too large $\alpha, \beta$ will leads to high stiffness

## Momentum

- extend from state $q$ to also include velocity related $\dot{q}$. (otherwise we need to use $q_1, q_2$ to describe the state)

One trival way: 

$$
\min_{q, v} \int_0^T L(q, v, t) dt \\
\text{s.t. } \dot{q} = v
$$

KKT condition:

$$
\partial_q \int L_d(q, v) + p^T (\dot{q} - v)  dt = 0 \\
\partial_v \int L_d(q, v) + p^T (\dot{q} - v)  dt = 0 \\
\dot{q} - v = 0
$$

integration by parts trick: (refer to E-L eqn derivation)

$$
\frac{\partial L}{\partial q} - \dot{p} = 0 \\
\frac{\partial L}{\partial v} - p = 0 \\
\dot{q} - v = 0
$$

Now we can see $p$ is the momentum. This is **Hamilton-Pontryagin principle** (another form of least action principle). 

## Hamiltonian Mechanics

Now, replacing v in KKT with $p$, we get:

$$
\int L + p^T (\dot{q} - v) dt = 
\int -0.5 p^T M^{-1} p -U + p^T v dt 
$$

Define **Hamiltonian**:

$$
H(q, p, t) = 0.5 p^T M^{-1} p + U(q)
$$

The new least action principle is:

$$
S(q, p, t) = \int_0^T - H(q, p, t) + p^T \dot{q} dt 
$$

Now the new KKT condition is (**Hamiltonian Equations**):

$$
\partial_q H + \dot{p} = 0 \\
\partial_p H - \dot{q} = 0 
$$

- in classical mechanics, $H$ is equivalent to $L$ least action principle. in quantum mechanics, $H$ is more common. 
- in optimization, this is called **Lagrangian Dual Problem**
- in physics, it is called **Legendre Transform**

### Legendre Transform

The object is to study the discrete form of Hamiltonian and how to integrate it. 

discrete form of Hamiltonian:

$$
p^+_{n+1} = \frac{\partial L_d(q_n, q_{n+1}, h)}{\partial q_{n+1}} \\
p^-_{n} = \frac{\partial L_d(q_{n+1}, q_n, h)}{\partial q_{n}} 
$$

Plugin $L_d = \frac{h}{2} (\frac{q_{n+1} - q_n}{h})^T M(\frac{q_{n+1} + q_n}{h}) - U(\frac{q_{n+1} + q_n}{2})$ into the Hamiltonian eqn, the discrete E-L eqn is:

$$
p^+_{n+1} - p^-_{n} = 0
$$

- Now the decision variable is on the knot points rather than the mid points. 



## Variational Integrator

- a specialized integrator for Lagrangian dynamics. (like RK-MK for quaternion)
- key idea: discrete E-L eqn instead of the ODE. then the constraint is also automatically held and conservative law is also satisfied.

Least action:

$$
\min_q S(q) = \int_0^T L(q(t), \dot{q}(t), t) dt
$$

discrete Lagrangian: 

$$
\int_{t_i}^{t_{i+1}} L(q(t), \dot{q}(t), t) dt \approx h L(\frac{q_i + q_{i+1}}{2}, \frac{q_{i+1} - q_i}{h}, t_i)
$$

dicreate E-L eqn:

$$
D_2 L_d(q_i, q_{i+1}, h) + D_1 L_d(q_{i+1}, q_{i+2}, h) = 0
$$

- "quadrature" approximation of the integral. 
- "implicit" midpoint method for ODE. 
- can take larger step size, more stable. 
- make sure the trajectory always lies in the energy manifold. 
- can be extended to higher-order integrator. 

### Variational Intergator / DEL with Constraints

Least action with constraints:

$$
\min_q S(q) = \int_0^T L(q(t), \dot{q}(t), t) dt \\
\text{s.t. } c(q(t)) = 0
$$

the new discrete E-L eqn:

$$
\partial_{q_{1:N}} \sum L_d(q_i, q_{i+1}, h) + h \lambda^T c(q_{i+1}) = 0
$$

- Note: the constraint is applied to the end point rather than the mid point to make sure it is satisfied. 

The same indexing trick, then we get (by removing the last and first term):

$$
D_2 L_d(q_i, q_{i+1}, h) + D_1 L_d(q_{i+1}, q_{i+2}, h) + h \lambda_{i} D c(q_{i+1}) = 0 \\
c(q_{i+2}) = 0
$$

- guarantee the constraint is satisfied

### Variational Integrator with Contacts

The least action with contact / non-conservative force:

$$
\min_q S(q) = \int_0^T L(q(t), \dot{q}(t), t) + F(t)^T q(t) dt \\
$$

the new discrete E-L eqn:

$$
\partial_{q_{1:N}} \sum L_d(q_i, q_{i+1}, h) + h F(t_i+\frac{h}{2})^T \frac{q_{i+1} + q_i}{2}  = 0
$$

- Now F(t) also need to be solved. In this case, force could be a function of position, velocity and time. 
