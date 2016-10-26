---
title: Hello Hexo
date: 2016-10-26 15:31:16
tags:
---

$a^2+b^2=c^2$

$$
a^2+b^2=c^2
$$


Inline:

Simple inline $a = b + c$.
Block:

$$\frac{\partial u}{\partial t}
= h^2 \left( \frac{\partial^2 u}{\partial x^2} +
\frac{\partial^2 u}{\partial y^2} +
\frac{\partial^2 u}{\partial z^2}\right)$$
Tag:

Single line content will be parsed as inline math (same as $...$):

This equation {% math %}\cos 2\theta = \cos^2 \theta - \sin^2 \theta =  2 \cos^2 \theta - 1 {% endmath %} is inline.
Multiple line content will be parsed as block math (same as $$...$$)

{% math %}
\begin{aligned}
\dot{x} & = \sigma(y-x) \\
\dot{y} & = \rho x - y - xz \\
\dot{z} & = -\beta z + xy
\end{aligned}
{% endmath %}
