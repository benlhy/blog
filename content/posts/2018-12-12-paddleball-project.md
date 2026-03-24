---
title: "Paddle-ball Project"
date: 2018-12-12T21:49:02+00:00
slug: "paddleball-project"
draft: false
description: "Modeling a bouncing triangular paddle-ball with a non-linear spring using Euler-Lagrange equations in Mathematica."
cover:
  image: "/images/2018/12/paddle.jpg"
  relative: false
  hiddenInList: false
tag: ["Mathematica", "Physics", "Machine", "Dynamics"]
---

This was a final project for a class in Machine Dynamics. The objective was to design a project that showcased all that we learned throughout the quarter. I decided to model a paddle-ball that utilized a strange sort of ball: a triangular ball. However, this would introduce some interesting behavior upon collision. The spring was also made discontinuous, so that only beyond a certain length would there be a spring constant, under this length, the spring acts like a string.

The behavior was modeled and solved using the Euler-Lagrange equations and Hamiltonian dynamics in Mathematica.

![](/images/2018/12/last.gif)

# Setting up the equations

A frame was assigned to each of the vertexes of the triangle and a set of transformation matrices was set up to convert the center-of-mass frame to the points on the vertex. The idea is that we want to detect collisions using the vertices, since the object was a triangle interacting with a line (our paddle). Due to numerical integration, we don't expect that two vertices would interact with the same line.

The constraint is the oscillating surface of the paddle, where the frequency is given by $f$: \[\phi = \text{sin}[ft]-y[t]\]

The kinetic energy of the system is given by the translational and rotational energy of the rigid body. The $V^b$ matrix of the body is determined by taking the inverse of the transformation matrix to the center of mass for the body multiplied by the time derivative of the transformation matrix, $g$:

\[V^b = g^{-1} \dot{g}\]

The potential energy is given by the sum of the gravitational potential energy of the ball and the potential energy stored in the discontinuous spring.

\[k=\begin{cases} 0, & \sqrt{x^2+(y-\sin[ft])^2}< 2 \\5, & \sqrt{x^2+(y-\sin[ft])^2}\geq 2.\end{cases}\]

Before collisions, the Euler Lagrange equations were set up as an unconstrained equations:\[\begin{align*}\frac{d}{dt}\frac{\delta L}{\delta \dot q} - \frac{\delta L}{\delta q} = 0\end{align*}\]

and collisions are detected when the y position of any vertex is equal to the y-vertex of the paddle.

## Collision

Once we have the point of intersection, we want to resolve the incoming velocity and solve the collision. We want the (\(x',y',\theta'\)) of the center of mass after the collision, and we know the configuration when it comes in, so we obtain the Hamiltonian before and after the collision, which should be the same, and solve for the generalized momenta with the constraint being the ground (paddle). The number of unknowns that we have to solve for is 4: (\(x,y,\theta,\lambda\)), and since we have 4 equations, we should be able to find a solution.

## Picking the right solution

Mathematica will return a set of suitable solutions.  We want $\lambda \leq 0$ because a positive value would mean that the paddle sucked the ball in. Picking the right solution to the update laws would then allow (\(x',y',\theta '\)) to be obtained after impact and the Lagrangian equations can be solved with no constraints until the next impact is detected.

## Comments

The Hamiltonian is not conserved because the paddle-ball is coupled to an external system and the spring is discontinuous.

Solving in Mathematica has some limitations because of the numerical analysis method of NDsolve. If x or \(\theta\) is set to zero initially, this will generate a complex infinity error. This is because the solution obtained from the Euler Lagrangian equations in free fall has a cot(\(\theta\)) term in it multiplied by a very small factor. In reality this might be zero, but when \(\theta = 0\), this will generate an infinity term, so I avoided this problem by giving a non-zero displacement to $x$ and \(\theta\).

If the frequency of the paddle is set to zero, after a few bounces, the system gains energy, and this might be due to the compounded error from numerical analysis as well as the discontinuous potential energy term.

The general behavior that was observed for this triangle ball is that it starts oscillating from side to side. Maybe that is why paddle balls are not made of triangular prisms.
