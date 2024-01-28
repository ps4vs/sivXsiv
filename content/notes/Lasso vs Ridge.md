---
title: Lasso vs Ridge
tags:
  - sapling
enableToc: false
---
### Introduction
Lasso and Ridge are regularisation methods used to find optimally complex model, which is as simple as possible while performing well on training data.

![[Pasted image 20240128033726.png]]
- Optimally complex model balances bias and variance.
### When Lasso When Ridge
- [Lasso]: To remove unnecessary features
 ![[Pasted image 20240128033627.png]]
- [Ridge]: To build robust model
![[Pasted image 20240128033645.png]]

### Feature Selection using Lasso
- Orange contour represents the regularisation term contour and blue contour represents the error term contour.
- The points where error term and regularisation terms are tangential to one another are the possible optimal solutions for which cost function can be minimised.
- The chances that two contours will be tangential to one another on x or y-axis are very unlikely to happen in Ridge, so it's difficult to have sparse solution.
- Since lasso has faces, corners, and sides in high-dimensions there are high chances that two contours are tangential to one another on x or y-axis.

![[Pasted image 20240128033122.png]]

![[Pasted image 20240128033437.png]]