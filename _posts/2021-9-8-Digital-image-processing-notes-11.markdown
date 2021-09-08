---
layout: post
title: "Digital Image Processing notes 11"
date: '2021-09-8 11:16 +0100'
author: Guan Heng
tags:
  - programming
  - typescript
  - tests
description: >-
  Digital Image Processing (CS/ECE 545) Lecture 11: Geometric Operations,Comparing Images.
image: /images/lena512x512.jpg
external:
  title: "Digital Image Processing notes 11"
  href: 
  status: at Quantum Tensors Medium
  date: 2021-9-8 11:16 +0100
---

## 1. Geometric Operations
- Filters, point operations change intensity
- Pixel position (and geometry) unchanged
- Geometric operations: change image geometry

![geometric operation example](/images/post-dip-11/geo_op_example.png)
$\color{red}{*Examples*}$: translating, rotating, scaling an image

**Definition**: Geometric operation transforms image $I$ to new image $I'$ by modifying coordinates of image pixels:

$$ I(x,y)\rightarrow I'(x',y')$$

Intensity value originally at (x,y) moved to new position (x’,y’)
![](/images/post-dip-11/geo_op_def.png)

- Since image coordinates can only be discrete values, some transformations may yield (x’,y’) that’s not discrete, **Solution**: interpolate nearby values

### Affine Mappings

* **Translation**:(shift)

$$\left\{ \begin{matrix} 1 & 0 & d_x\\ 0 & 1 & d_y \end{matrix} \right\}$$

![translation](/images/post-dip-11/translation.png)

* **scaling**:(contracting or stretching)

$$\left\{ \begin{matrix} s_x & 0 & 0\\ 0 & s_y & 0 \end{matrix} \right\}$$

![translation](/images/post-dip-11/scaling.png)

* **shearing**:

$$\left\{ \begin{matrix} 1 & b_x & 0\\ b_y & 1 & 0 \end{matrix} \right\}$$

![translation](/images/post-dip-11/shearing.png)

* **rotation**:

$$\left\{ \begin{matrix} \cos{\alpha} & -\sin{\alpha} & 0\\ \sin{\alpha} & \cos{\alpha} & 0 \end{matrix} \right\}$$

![translation](/images/post-dip-11/rotation.png)

* **flipping**:
  - To flip an image upside down:
    At pixel location $(x,y)$, look up the color at location $(x,(1–y))$
  - For horizontal flip:
    At pixel location $(x,y)$, look up $((1 – x),y)$

* **warping**:
  - Image warping: we can use a function to select which pixel somewhere else in the image to look up
  - For example: apply function on both texel coordinates (x, y)

  $$x=x+y*\sin(\pi*x)$$

  ![warping](/images/post-dip-11/warping.png)

* Affine (3‐Point) Mapping property
    - straight lines ‐> straight lines,
    - triangles ‐> triangles
    - rectangles ‐> parallelograms
    - Parallel lines ‐> parallel lines
    - Distance ratio on lines do not change

### Non‐Linear Image Warps
![non-linear warp](/images/post-dip-11/non-linear-warp.png)