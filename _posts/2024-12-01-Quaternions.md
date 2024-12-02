---
layout: post
title: Quaternions
date: 2024-12-01 12:00:00
description:
tags:
categories:
featured: false
---

<br> 
### **Introduction** <br>
<br> 

In computer graphics, rotations are commonly represented using matrices. For example, a rotation by an angle $$\theta$$ around the axis $$A$$ is expressed using the following matrix: 

\begin{bmatrix}
\cos\theta & -\sin\theta & 0\\
\sin\theta & \cos\theta & 0\\
0 & 0 & 1
\end{bmatrix}

Applying this rotation to a vertex is as simple as multiplying the vertex by the matrix.

However, matrices are not the only way to represent rotations. There is a better alternative: **Quaternions**.

In this post, we’ll dive deep into quaternions.

We will explore their mathematical foundations, their use in representing rotations, and how they can be more easily interpolated to produce smooth animations.
