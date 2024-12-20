---
title: "Multibody Dynamics Simulation"
date: 2024-12-07
draft: false
author: "Pushkar Dave"
tags:
  - Python
  - Dynamics
  - Lagrangian Mechanics
image: 
description: This project involves modeling a jack in a box with Lagrangian Mechanics
toc: true
mathjax: true
---

### Demo Video
This project involved modeling a Jack and a Box and the impacts between them using Lagrangian Mechanics. 

{{<youtube 3SNTRruWNic>}}

---
### Frames and Transformations

The image below depicts the frames used for modeling the dynamics of the system. There are in total **11** frames, the `World` frame, the `Box` frame at the box's geometrical center and the `Jack` frame at the jack's geometrical center. To accurately check for impacts, there are **8** additional frames, **4** for each corner of the Jack, labelled `r1` through `r4` and **4** for each side of the box (located at the center of each side), labelled  `p1` through `p4`.
![image](./Frames.png)

The configuration of the system is represented using **6** configuration variables `xBox, yBox, thBox, xJack, yJack, thJack`.  The variables `xBox, yBox, thBox` represent the pose of the box with respect to the *world* frame and the variables `xJack, yJack, thJack` represent the pose of the jack with respect to the *box* frame.

Math equations can be rendered using [Mathjax](https://www.mathjax.org) syntax with AMS symbol support.

Optionally enable this on a per-page basis by adding `mathjax: true` to your frontmatter.

Then, use `$$ ... $$` on a line by itself to render a block equation:

$$ | Pr_{x \leftarrow P_{1}} [A(x) = 1] - Pr_{x \leftarrow P_{2}} [A(x) = 1] | < \text{negligible} $$

The raw version is:

```
$$ | Pr_{x \leftarrow P_{1}} [A(x) = 1] - Pr_{x \leftarrow P_{2}} [A(x) = 1] | < \text{negligible} $$
```


Write in-line equations with `\\( ... \\)` , like \\( x^n / y \\) . It's easy!

```
Write in-line equations with `\\( ... \\)` , like \\( x^n / y \\) . It's easy!
```
