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

When a beam of light hits the surface of an object, part of its energy is absorbed by the surface, part of its energy is reflected away and part of its energy may refract through the object itself. 

In this post, we will explore the mathematics behind calculating the refraction vector. 

<br> 
### Snell Law <br>

Transparent surfaces possess a property called the **index of refraction**. This refractive index determines how much the path of light is bent or refracted, when entering a material. This can be explained by **Snell's Law**. 

Let:

- $$n_L$$ be the index of refraction of the material the light is leaving, 
- $$\theta_L$$ be the angle of incidence, 
- $$n_T$$ be the index of refraction of the material the light is entering, and
- $$\theta_T$$ be the angle of refraction. 

According to Snell's Law, 

$$
n_L sin \theta_L = n_T sin \theta_T
$$

Each material has its own unique index of refraction. For example, the index of refraction of air is **1.000293**, while the index of refraction of diamond is **2.417**. Higher indexes of refraction create a greater bending effect at the interface between two materials, causing the refraction vector to bend more towards the normal vector.

<br> 
### Decomposition of Vectors<br> 

To calculate the refraction vector, we first need to decompose the incoming light vector $$L$$ in relation to the surface normal vector 
$$N$$

We assume that the normal vector $$N$$ and the incoming light $$L$$ has been normalized to unit length. 

Each vector has both parallel component and perpendicular component relative to the normal $$N$$. 
  
The parallel component of $$L$$ along $$N$$ is 

$$
L_{||} = (N.L)N
$$

Alternatively, this can be expressed as $$N cos \theta$$

The perpendicular component of $$L$$ along $$N$$ is 

$$
L_⊥ = L - L_{||} = L - (N.L)N
$$

<br>
### Calculating Component Magnitudes <br>

Next, let's calculate the magnitudes of $$ L_{||} $$ and $$ L_⊥ $$

Since the vectors $$L$$, $$ L_{||} $$ and $$ L_⊥ $$ form a right-angled triangle, we can use trignometric relationships: 

$$
sin\theta_L = \frac{|L_⊥|}{L}
$$ 

and 

$$
cos\theta_L = \frac{|L_{||}|}{L}
$$ 


Since $$L$$ has been normalized to unit length (i.e., $$ |L| = 1 $$), then 

$$
|L_⊥| = sin \theta_L
$$

and 

$$
|L_{||}| = cos \theta_L
$$
