---
layout: post
title: Refraction Vector Calculation
date: 2024-10-31 12:00:00
description:
tags:
categories: Computer-Graphics
featured: false
---

### Introduction <br>

When a beam of light hits the surface of an object, part of its energy is absorbed by the surface, part of its energy is reflected away and part of its energy may refract through the object itself. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/REFRACTION0.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

In this post, we will explore the mathematics behind calculating the refraction vector. 

<br> 
### Snell Law <br>

Transparent surfaces possess a has called the **index of refraction**. This refractive index determines how much the path of light is bent or refracted, when entering a material. This can be explained by **Snell's Law**. 

Let:

- $$n_L$$ be the index of refraction of the material the light is leaving, 
- $$\theta_L$$ be the angle of incidence, 
- $$n_T$$ be the index of refraction of the material the light is entering, and
- $$\theta_T$$ be the angle of refraction. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/REFRACTION1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

According to Snell's Law, 

$$
n_L sin \theta_L = n_T sin \theta_T
$$

Each material has its own unique index of refraction. For example, the index of refraction of air is **1.000293**, while the index of refraction of diamond is **2.417**. Higher indexes of refraction create a greater bending effect at the interface between two materials, causing the refraction vector to bend more towards the normal vector.

<br> 
### Decomposition of Incoming Light Vector $$L$$ <br> 

To calculate the refraction vector, we first need to decompose the incoming light vector $$L$$ in relation to the surface normal vector 
$$N$$. 

We assume that the normal vector $$N$$ and the incoming light $$L$$ has been normalized to unit length. 

Each vector has both parallel component and perpendicular component relative to the normal $$N$$. 
  
The parallel component of $$L$$ along $$N$$ is 

$$
L_{||N} = (N.L)N
$$

Alternatively, this can be expressed as $$N cos \theta$$

The perpendicular component of $$L$$ along $$N$$ is 

$$
L_{⊥N} = L - L_{||N} = L - (N.L)N
$$

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/REFRACTION2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/REFRACTION3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Next, let's calculate the magnitudes of 
$$L_{||}$$
and 
$$L_⊥$$

Since the vectors 
$$L$$
, 
$$L_{||}$$ 
and
$$L_⊥$$ 
form a right-angled triangle, we can use trignometric relationships: 

$$
sin\theta_L = \frac{|L_⊥|}{L}
$$ 

and 

$$
cos\theta_L = \frac{|L_{||}|}{L}
$$ 


<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/REFRACTION4.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Since $$L$$ has been normalized to unit length (i.e., 
$$|L| = 1$$
), then 

$$
|L_⊥| = sin \theta_L
$$

and 

$$
|L_{||}| = cos \theta_L
$$

Now, we are ready to calculate the refraction vector $$T$$. 

Just like we did with the incoming vector $$L$$, we are going to decompose the refraction vector $$T$$ in relation to the surface normal vector $$N$$. 

The parallel component of $$T$$ along $$N$$ is 

$$
T_{||} = (-N.T)(-N)
$$

We can calculate the perpendicular component of $$T$$ along $$N$$ b 

$$
T_⊥ = T - T_{||}
$$

However, we do know $$T$$. Hence, we must find another way to calculate $$T_⊥$$. 

We know that $$T_⊥$$ has the same direction as $$L_⊥$$. We also know that the magnitude of $$T_⊥$$ is equal to $$sin \theta_T$$. Therefore, we can calculate $$T_⊥$$ as: 

$$
T_⊥ = normalized($$L_⊥$$) sin \theta_T
$$
