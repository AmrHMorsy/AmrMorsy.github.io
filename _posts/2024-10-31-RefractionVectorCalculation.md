---
layout: post
title: Refraction Vector Calculation
date: 2024-10-31 12:00:00
description:
tags:
categories:
featured: false
---

### Introduction <br>

When a beam of light hits the surface of an objecy, part of its energy is absorbed by the surface, part of its energy is reflected away and part of its energy may refract through the object itself. 

In this blog post, we will discuss the mathematics of how to calculate the refraction vector. 

<br> 
### Snell Law <br>

Transparent surfaces possess a property called the **index of refraction**. This refractive index determines how much the path of light is bent or refracted, when entering a material. This can be explained by **Snell's Law**. 

Let $$n_L$$ be the index of refraction of the material the light is leaving, $$\theta_L$$ be the angle of incidence, $$n_T$$ be the index of refraction of the material the light is entering and $$\theta_T$$ be the angle of refraction. **Snell Law** states that: 

<br>
$$
n_L sin \theta_L = n_T sin \theta_T
$$
<br>

Each medium has its own index of refraction. For example, the index of refraction of air is 1.000293, while the index of refraction of diamond is 2.417. Higher indexes of refraction create a greated bending effect at the interface between two materials; the refraction vector bends more towards the normal vector. 

<br> 
### Decomposition of Vectors<br> 

We assume that the normal vector $$N$$ and the incoming light $L$ has been normalized to unit length. 

Each vector $$V$$ has both parallel component relative to $$N$$, $$V_{||}$$, and perpendicular component relative to $$N$$, $$V_⊥$$ ; 
  
The parallel component of $$L$$ along $$N$$ is 

$$
L_{||} = (N.L)N
$$

Since 

$$ 
L = L_{||} + L_⊥
$$

then the perpendicular component of $$L$$ along $$N$$ is 

$$
L_⊥ = L - (N.L)N
$$

Also, we need to determine the length of the $$L_⊥$$, $$|L_⊥|$$. We will later use it in the equation of the refraction vector. 

We will use the concept of the right-angled triangle: 

Since 

$$
sin\theta_L = \frac{|L_⊥|}{L}
$$ 

then, 

$$
|L_⊥| = |L| sin \theta_L
$$

Since $$L$$ has been normalized to unit length, then 

$$
|L_⊥| = sin\theta_L
$$
