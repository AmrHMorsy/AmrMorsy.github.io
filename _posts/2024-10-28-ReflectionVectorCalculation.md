---
layout: post
title: Reflection Vector Calculation
date: 2024-10-28 12:00:00
description:
tags:
categories:
featured: false
---


### Introduction <br>

About a month ago, I was interviewed for a graphics programmer position. Among the questions asked was how to calculate the reflection vector of an incident light ray on a surface. Unfortunately, I was a bit nervous during the interview and couldn’t think clearly, so I couldn’t answer the question. Unsurprisingly, I didn’t get the position.

Ironically, the solution came to mind shortly after the interview. I took some time to review the math behind it again and decided to write a blog post to break it down.

In this post, I'll explain the mathematics of calculating the reflection vector given an incident light vector on a surface. Let $$L$$ be the incident light vector at an angle $$\theta$$ from the normal, $$N$$, of the surface, and $$R$$ be the reflection vector of $$L$$. 

Suppose $$N$$ and $$L$$ are normalized to unit length.

<br>
### Law of Reflection <br>

The law of reflection states that the angle of incidence is equal to the angle of reflection. Thus, we know that the reflection angle is also $$\theta$$ from the normal, $$N$$. 

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/D1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
### Decomposition of Vectors <br>

Each vector has both parallel and perpendicular components relative to $$N$$. 

The parallel component of $$L$$ along $$N$$ is: 

$$
L_{||} = (L.N)N
$$

Alternatively, this can be expressed as $$N cos \theta$$

The perpendicular component of $$L$$ along $$N$$ is: 

$$
L_⊥ = L - L_{||} = L - (L.N)N
$$

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/D2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
### Calculating the Reflection Vector <br>

To compute $$R$$, we reverse the parallel component of $$L$$ along $$N$$ and keep the perpendicular component unchanged:

$$
R = L_⊥ - L_{||} 
$$

$$
R = L - (L.N)N - (L.N)N 
$$

$$
R = L - 2 (L.N)N
$$

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html loading="eager" path="assets/img/D3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<br>
### Implementation <br>

Here’s the C++ code using the GLM library:

```c++
glm::vec3 compute_reflection_vector( glm::vec3 L, glm::vec3 N )
{
    return L - 2.0f * glm::dot( L, N ) * N ;
}
```
