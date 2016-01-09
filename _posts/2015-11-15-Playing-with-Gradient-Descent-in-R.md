---
layout: post
title: Playing with Gradient Descent in R
date: 2015-11-15
author: bryan
comments: true
categories: [Articles]
noindex: false
published: false
---

One of the first algorithms that Andrew Ng discusses in his [Coursera Machine Learning course](https://www.coursera.org/course/ml) is gradient descent. He continues to use it throughout the course, but his initial example is simple and a good place to start. In this example, it is used to minimize the cost function (the sum of squared errors or SSE) for obtaining parameter estimates for a linear model. I.e.:

$$
\text{minimize } J(\theta_0, \theta_1) = \dfrac {1}{2m} \displaystyle \sum _{i=1}^m \left (h_\theta (x^{(i)}) - y^{(i)} \right)^2
$$

Which, when applied to a linear model becomes:

$$
\begin{align*} \theta_0 := & \theta_0 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m}(h_\theta(x^{(i)}) - y^{(i)}) \\
\\
\theta_1 := & \theta_1 - \alpha \frac{1}{m} \sum\limits_{i=1}^{m}\left((h_\theta(x^{(i)}) - y^{(i)}) x^{(i)}\right) \end{align*}
$$

Where $\theta_0$ is our intercept and $\theta_1$ is the parameter estimate of our only predictor variable.

Ng's course is Octave-based, but manually calculating the algorithm in an R script is a fun, simple exercise and if you're primarily an R-user it might help you understand the algorithm better than the Octave examples. The code full code is in this repository, but here is the walkthrough:

* Create some linearly related data with known relationships
* Write a function that takes the data and starting (or current) estimates as inputs
* Calculate the cost based on the current estimates
* Adjust the estimates in the direction and magnitude indicated by the scaling factor $\alpha$.
* Recursively run the function, providing the new parameter estimates each time
* Stop when the estimate converges (i.e., meets the stopping criteria based on the change in the estimates)

This code is for a simple single variable model. Adding additional variables means calculating the partial derivatives with respect to each item. In other words, adding a version of the $\theta_1$ cost component for each feature in the model. I.e.,

$$
\begin{align*}
\theta_j := & \theta_j - \alpha \frac{1}{m} \sum\limits_{i=1}^{m}\left((h_\theta(x^{(i)}) - y^{(i)}) x_j^{(i)}\right) \end{align*}
$$



Feel free to fork and improve. Incidentally, I use Gradient Descent as a 'Hello World' program when I'm playing with statistical packages. There a few implementations in this repo(link). Note, not all of them are necessarily optimized or even correct. Use them only for getting an idea of how things would work.
