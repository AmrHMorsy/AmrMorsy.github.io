---
layout: post
title: Cloth Simulation using Mass-Spring Systems - Theory and Implementation
date: 2024-05-13 15:09:00
description:
tags:
categories: 
featured: false
---

Physically-based deformable model animation has been an active area of computer graphics for over two decades, marked by significant contributions such as Lasseter's exploration of dynamic expressions through 'squash and stretch' [Las87] and Terzopoulos et al.'s seminal work on elastically deformable models [TPBF87]. This field combines concepts from Newtonian dynamics, continuum mechanics, numerical computation, differential geometry, vector calculus, and approximation theory.

Cloth simulation is one of the subfields within physically-based deformable model animation. It has attracted extensive research aiming at developing algorithms that achieve both visual and physical accuracy while maintaining computational efficiency. Researchers have experimented with various physical models for these applications, including mass-spring systems, finite elements, mesh-free methods, coupled particle systems, and reduced deformable models. Each of these models have their own unique strengths and challenges.
 
In this blog, we will focus on implementing cloth simulation using the mass-spring system. This system is simpler and easier to implement than more physically consistent models derived from continuum mechanics, such as the finite element method. The mass-spring system is a widely used method to simulate hair, cloth and, to a lesser extent, elastic solids. Although it is not the most accurate technique, it is fast and stable, making it ideal for real-time applications like video games, where speed is often prioritised over precision.
 
We will begin by exploring various time integration methods used in simulating cloth with mass-spring systems. We will start with the explicit Euler and implicit Euler methods. Next, we will introduce a new, faster, and more stable time integration scheme that uses a solver based on the block coordinate descent method. This technique is detailed in the paper 'Fast Simulation of Mass-Spring Systems,' and it will be the basis for our implementation in the cloth simulation project. Finally, we will showcase the cloth animation in different scenes and assess its accuracy and speed.
