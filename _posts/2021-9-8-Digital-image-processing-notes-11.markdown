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

<font color=red size=4>*Examples*</font>: translating, rotating, scaling an image

**Definition**: Geometric operation transforms image $I$ to new image $I'$ by modifying coordinates of image pixels:

$$ I(x,y)\rightarrow I'(x',y')$$

Intensity value originally at (x,y) moved to new position (x’,y’)
![](/images/post-dip-11/geo_op_def.png)

- Since image coordinates can only be discrete values, some transformations may yield (x’,y’) that’s not discrete, **Solution**: interpolate nearby values

### Affine Mappings

* **Translation**:(shift)

$$\left\{ \begin{matrix} 1 & 0 & d_x\\ 0 & 1 & d_y \end{matrix} \right\}$$

<div align=center>
![translation](/images/post-dip-11/translation.png)
</div>

* **scaling**:(contracting or stretching)

$$\left\{ \begin{matrix} s_x & 0 & 0\\ 0 & s_y & 0 \end{matrix} \right\}$$

<div align=center>
![translation](/images/post-dip-11/scaling.png)
</div>

* **shearing**:

$$\left\{ \begin{matrix} 1 & b_x & 0\\ b_y & 1 & 0 \end{matrix} \right\}$$

<div align=center>
![translation](/images/post-dip-11/shearing.png)
</div>

* **rotation**:

$$\left\{ \begin{matrix} \cos{\alpha} & -\sin{\alpha} & 0\\ \sin{\alpha} & \cos{\alpha} & 0 \end{matrix} \right\}$$

<div align=center>
![translation](/images/post-dip-11/rotation.png)
</div>

* **flipping**:
  - To flip an image upside down:
    At pixel location $(x,y)$, look up the color at location $(x,(1–y))$
  - For horizontal flip:
    At pixel location $(x,y)$, look up $((1 – x),y)$

* **warping**:
  - Image warping: we can use a function to select which pixel somewhere else in the image to look up
  - For example: apply function on both texel coordinates (x, y)

  $$x=x+y*\sin(\pi*x)$$
  <div align=center>
  ![warping](/images/post-dip-11/warping.png)
  </div>

* Affine (3‐Point) Mapping property
    - straight lines ‐> straight lines,
    - triangles ‐> triangles
    - rectangles ‐> parallelograms
    - Parallel lines ‐> parallel lines
    - Distance ratio on lines do not change

### Non‐Linear Image Warps
<div align=center>
![non-linear warp](/images/post-dip-11/non-linear-warp.png)
</div>

* **twirl**:
<div align=center>
![twirl](/images/post-dip-11/twirl.png)
</div>

* **ripple**:
<div align=center>
![ripple](/images/post-dip-11/ripple.png)
</div>

* **Spherical Transformation**:
<div align=center>
![spherical-transform](/images/post-dip-11/spherical_trans.png)
</div>

---
## 2.Template Matching
Distance between image patterns
  - sum of absolute differences:
  $$ d_A(r,s)=\sum_{(i,j)\in{R}}|I(r+i,s+j)-R(i,j)|$$

  - Maximum differences:
  $$ d_M(r,s)=\max_{(i,j)\in{R}}|I(r+i,s+j)-R(i,j)|$$

  - sum of squared differences(also called M-dimensional Euclidean distance):
  $$ d_E(r,s)=[\sum_{(i,j)\in{R}}(I(r+i,s+j)-R(i,j))^2]^{\frac{1}{2}}$$

  - **Best matching Distance and Correlation**:
  <div align=center>
  ![best-matching-dist and corr](/images/post-dip-11/dist_format.png)
  </div>
  B term is a constant, independent of r,s and can be ignored, A term is sum of squared valueds within subimage / at current offset r,s.
  C(r,s)term is *linear cross correlation* between I and R defined as:

  $$
  (I \times{R})(r,s)=\sum_{i=-\infty}^{\infty}\sum_{j=-\infty}^{\infty}I(r+i,s+j)\cdot R(i,j)
  $$

  Since R and I are assumed to be zero outside their boundaries:
  
  $$
  \sum_{i=0}^{w_R-1}\sum_{j=0}^{h_R-1}I(r+i,s+j)\cdot R(i,j)=\sum_(i,j\in R)I(r+i,s+j)\cdot R(i,j)
  $$

  **Note:** Correlation is similar to linear convolution
  Min value of $d^2_E(r,s)$ corresponds to max value of $(I\times R)(r,s)$

  <div align=center>
  ![norm-scoss-corr](/images/post-dip-11/norm_cross_corr.png)
  </div>

  <div align=center>
  ![corr-coef](/images/post-dip-11/corr_coef.png)
  </div>

  - Correlation coefficient Algorithm
  <div align=center>
  ![corr-coef-algorithm](/images/post-dip-11/corr_coef_algorithm.png)
  </div>