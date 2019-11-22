---
title: Robotic Hand Gripper
author: ~
date: '2019-10-11'
slug: robotic-hand-gripper
categories: []
tags: []
description: "I print a gripper and make him move with arduino" 
---

{{< fa folder-open >}} [repo](https://github.com/amaurylekens/roboticHandGripper)

# Printing of the robotic hand

$$v = \begin{bmatrix}
v_1 \\
v_2
\end{bmatrix}
\qquad
\qquad
w = \begin{bmatrix}
w_1 \\
w_2
\end{bmatrix}
\qquad\qquad
v + w = \begin{bmatrix}
v_1 + w_1 \\
v_2 + w_2
\end{bmatrix}$$

The print of the robotic hand was very simple. I just retrieved a template from the site thingiverse. We can find the print files [here](https://www.thingiverse.com/thing:1748596).

# Control of the robotic hand

The arm integrates 6 servo motor and a step motor. To control these engines we use an arduino mega.

To separate the power of the control signals, I made an electronic card with [L293DNE](http://www.ti.com/lit/ds/symlink/l293.pdf) chips. This card also simplifies the connections between the arduino and the different motors.

![function_1](/motor_card.jpg#center)