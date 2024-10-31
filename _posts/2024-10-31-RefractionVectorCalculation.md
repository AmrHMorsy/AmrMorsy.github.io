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

### Snell Law

Transparent surfaces possess a property called the index of refraction. This refractive index determines how much the path of light is bent or refracted, when entering a material. This is explained by Snell's Law, which states that: 

$$
n_L sin \theta_L = n_T sin \theta_T
$$

where

$$n_L$$ is the index of refraction of the material the light is leaving 
$$\theta_L$$ is the angle of incidence 
$$n_T$$ is the index of refraction of the material the light is entering
$$\theta_T$$ is the angle of refraction

Each medium has its own index of refraction. For example, the index of refraction of air is 1.000293, while the index of refraction of diamond is 2.417. Higher indexes of refraction create a greated bending effect at the interface between two materials; the refraction vector bends more towards the normal vector. 
