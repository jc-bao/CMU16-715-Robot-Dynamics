## Calculus of Variations

Basic idea: [reference video](https://www.youtube.com/watch?v=SQLxrr9N8zM)
- function of functions = functional. 
- standard calculus: study functions that map real numbers to real numbers. (solve for $x$)
- variational calculus: study functionals that map functions to real numbers. (solve for $f$)
- from another perspective, $f$ is acually infinite number of variables. 
- the "gradient" (variation) of functional is still a function. 

Concept:
- variation: $\delta f$
- variation of functional: $\delta F[f] = F[f + \delta f] - F[f]$, real number change given a change in function. 

Example: hanging chain between two points. Objective is to find the shape of the chain $f(x)$. 
- cable length: $L = \int_0^1 \sqrt{1 + f'(x)^2} dx$
- potential energy: $V = \int_0^1 f(x) \sqrt{1 + f'(x)^2} \rho g dx$
- minimize potential energy: 


## Euler-Lagrange Equation

The solution to: ($q$ is the function)

$$
\min_q J(q) = \int_0^T L(q, \dot{q}, t) dt \\ 
\text{s.t. } \int_0^T c(q, \dot{q}, t) dt = 0
$$

is given by: (proof with KKT condition. [video](https://youtu.be/GPgx7J6L0sk?list=PLZnJoM76RM6ItAfZIxJYNKdaR_BobleLY&t=3716))

$$
\partial_q L - \frac{d}{dt} \partial_{\dot{q}} L + \lambda (\partial_q c - \frac{d}{dt} \partial_{\dot{q}} c) = 0
$$


- intuition: convert global property (like minimizing energy) to local property (consistency between neighboring points). Very similar to dynamic programming. 
- 🌟 relation to DP, RL: this idea that local optimality leads to global optimality is very powerful. This is called the Princeple of Optimality of Bellman's Principle of Optimality. 
- 🌟 relation to optimal control: L -> cost, c-> dynamics. q -> state, $\dot{q}$ -> control. (note: here the control is the velocity, a special control system. )