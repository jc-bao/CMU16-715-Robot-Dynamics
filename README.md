# CMU-16-715-Robot-Dynamics
Lecture notes for CMU 16715 Advanced Robot Dynamics. 

## Overview

- Describe a system: Newton-Euler, Lagrangian, and Hamiltonian
- Simulate a system: discretization, integration, SO(3)
- Contact: simulate contact and friction. 
- Understand simulator: why it diverges, why it is not deterministic, why the contact not act the way we want, what could be the sim2real gap. 

## Outline

### Basics: discretization, integration, SO(3)

- Lecture 1: A Brief History of Dynamics, Basics of Newtonian Mechanics
- Lecture 2: state space, euler integration, energy, Lyapunov stability (tell if simulation would diverge.)
- Lecture 3: Taylor integration, Runge-Kutta integration (achieve higher-order integration without calculating the higher-order gradient. ), Implicit midepoint. 
- Lecture 4: Higher-order RK Method, Stiffness + Stability of RK methods. 

### Single Rigid Body Dynamics

- Lecture 5: Rigid Body, Reference Frames, Attitude Representation, Rotation Matrix. 
- Lecture 6: Linear Systems, Group Theory, Rotation Matrix Kinematics, Quaternion Geometry. 
- Lecture 7: Kinematic Energy, Intertia, Euler's Equation (F=ma in rotation form)
- Lecture 8: Stability of spinning rigid body, numerical simulation of 3d Rotations, 
- Lecture 9: Newton-Euler dynamics, SE3, Quadrotor, Airplane

### Optimization Recap [Notebook](./math.md)

- Lecture 10: Root finding, Minimization. 
- Lecture 11: Constrained Minimization, Equality Constraints, Inequality Constraints. 

### Lagrangian Dynamics [Notebook](./lagrangian.md)

- Lecture 12: Calculus of Variations, Euler-Lagrange Equation.
- Lecture 13: Dynamics from Energy, Lagrangian Mechanics, Least-action principle. 
- Lecture 14: Interpretation of Least Action, Manipulator Equation, Non-convervative, Contraints, Coordinates. 
- Lecture 15: Simulation with Constraints, Differential Algebraic Equations, Variational Integrator.
- Lecture 16: Momentum, Legendre Transform, Hamiltonian Mechanics
- Lecture 17: Discrete Legendre Transform, Variational Integrator with Constraints. 

### Contact Dynamics [Notebook](./contact.md)

- Lecture 18: Contact Dynamics, Discrete Mechanics with Impacts. 
- Lecture 19: Coulomb Friction, Maximum Dissipation Principle, LCP Methods. 

### Applications and Extensions

- Lecture 20: Fixed-based Manipulators, Forward Kinematics, Floating-based Systems 
- Lecture 21: Kinematics in 3D, Least-action for rigid body, Floating-based robot dynamics. 
- Lecture 22: Floating-based dynamics in maximal coordinates, variational integrators in maximal coordinates. 
- Lecture 23: "Fast" Dynamics Algorithms


## Resources

[Course Syllabus](https://github.com/dynamics-simulation-16-715/syllabus/blob/main/syllabus.pdf)

[Lecture Notebooks](https://github.com/dynamics-simulation-16-715/lecture-notebooks/tree/main)

[Video Lectures](https://www.youtube.com/watch?v=LiNgr1tz49I&list=PLZnJoM76RM6ItAfZIxJYNKdaR_BobleLY&index=1)

[Physics-Based Simulation](https://phys-sim-book.github.io/)

[Mujoco Offical Documentation](https://mujoco.readthedocs.io/en/stable/overview.html)
