---
doi: 10.yourblog20260306132
title: 'A Tour of Functional Itô Calculus ala Dupire'
subtitle: 'Learn why do they really matter in finance'
draft: true

# Primary categorization (1-3 max)
categories:
  - "functionals"
  - "functional integrals"
  - "Path Integral"


# Specific tags (5-10, SEO + discoverability)
tags:
  - "Wiener Integral"
  - "Measure Theory"
  - "Path Integrals"
  - "Feynman Integral"
  - "Functional Derivative"

# Book series (for multi-part articles)
series: ["Quant Book"]

# Rendering flags (your MathJax setup)
math: true
toc: true

# PaperMod feature image (from your generated cover)
cover:
  image: "/images/posts/infinitesimal-generator.png"  # ← Exact path
  alt: "Itô diffusion generator visualization"
  caption: "Generator and Feynman-Kac duality"

# SEO / discoverability
description: "Explore Functional Itô Calculus and Bruno Dupire's path-dependent framework. Understand functional derivatives and their application to volatility surface modeling and path-dependent options."
keywords: ["Functional Itô Calculus", "Dupire", "Path-dependent derivatives", "Vertical derivative", "Horizontal derivative", "Quantitative Finance"]

# Optional: social sharing
twitter: "quantatrisk"
github: "quantatrisk/quantatrisk.github.io"
---
doi: 10.yourblog20260306132

## What is a functional?    

One says that a **functional** is given if a rule is fixed that associates a number to every function from certain function family. Thus, it is a **mapping** from a set of functions to a set of numbers. It is a **generalization** of the idea of a function of several variables to the realm of a *function of infinitely many variables*, the values of the argument function. For example, $sin(x^2)$ is not a functional. On the other hand

$$
\begin{equation}  \tag{1.1}
I_1[f]=\int_{0}^{1}f(x)dx 
\end{equation}
$$

is a functional of $f$; it assigns a numerical value to the entire function, $f$. It is an analog of the finite sum $\sum f_i$.

Functionals can simultaneously be functions of a numerical variable. One example of how this can occur is

$$
\begin{equation}  \tag{1.2}
I_2[f;t]=\int_{0}^{1}g(x,t)f(x)dx 
\end{equation}
$$

where $g$ is a given function of two variables. For fixed $t$ it is a functional of $f$ of the same type as $I_1$. Now let $f(x,t)$ be a function of two variables and consider

$$
\begin{equation}  \tag{1.3}
I_3[x,t]=\int_{0}^{1}g(x)f(x,t)dx 
\end{equation}
$$

where $g$ is again some fixed function. This is another way that a functional can be an ordinary function at the same time. There are many other examples that could be invented of how this might occur.

Not all functionals are defined by integrals. The simplest example of one that is not is the Dirac delta 'function, $\delta (x)$. This is often described as the function that is zero everywhere except at the origin, where it is infinite in such a way that

$$
\begin{equation}  \tag{1.4}
\int_{-\infty}^{\infty}\delta (x)dx=1. 
\end{equation}
$$

Of course, there is no function for any reasonable definition of the integral sign. The delta 'function' is really a 'functional', assigning to any function, its value at origin. We may write

$$
\begin{equation}  \tag{1.5}
\delta [f]=f[0]. 
\end{equation}
$$

This is what is really meant by the usual notation

$$
\begin{equation}  \tag{1.6}
\int_{-\infty}^{\infty}f(x)\delta (x)dx=f(0). 
\end{equation}
$$

There is a differential and integral calculus for functions of finitely many variables. *Is there an analog for 'functionals'*? Function spaces are huge and, in general it is not possible to define measures on them in a way that would make integral calculus possible. But there are exceptional special cases.  The **Wiener integral** is an integral of a functional over the space of continuous functions on an interval, albeit with a very **special measure**. The **Feynman integral** is also an integral of a *functional* over a **function space**. On the other hand, differentiation of functionals is possible. Suppose, we take a functional $I[f]$, and ask how it changes when we make a small change in $f$, to $f+\delta f$. This is the result of changes at all different values of $x$ in the domain of $f$ so we write it as 

$$
\begin{equation}  \tag{1.7}
\delta I[f,df]=\int \frac{\delta I[f]}{\delta f(y)}\delta f(y)dy. 
\end{equation}
$$

The quantity $\frac{\delta I[f]}{\delta f(y)}$ is called the **functional derivative** of $I$ with respect to $f$. It is the coefficient of $\delta f$ in the linear part of the change in $I$. Equation (1.7) is analogous to the formula for the differential of a function of several variables $\delta f=\sum \left ( \partial f/\partial x_i \right )\delta x_i$. In general, the functional derivative $\frac{\delta I[f]}{\delta f(y)}$ is both a functional of $f$ and an ordinary function of $y$.

We now want to see how a functional changes as you adjust the function which is fed into it. Recall that a derivative of a function is defined as follows:

$$
\begin{equation}  \tag{1.8}
\frac{dI}{dx}=\displaystyle \lim_{\epsilon \to 0}\frac{I(x+\epsilon)-I(x)}{\epsilon}
\end{equation}
$$ 

The derivative of the function tells you how the number returned by the function $I(x)$ changes as you slightly change the number $x$ that you feed into the ‘machine’. In the same way, we can define a **functional derivative** of a functional $I[f]$ as follows

$$
\begin{equation}  \tag{1.9}
\frac{\delta I}{\delta f(x)}=\displaystyle \lim_{\epsilon \to 0}\frac{I[f(x')+\epsilon \delta (x-x')-I[f(x')]}{\epsilon}
\end{equation}
$$

The functional derivative tells you how the number returned by the functional $I[f(x)]$ changes as you slightly change the function $f(x)$ that you feed into the machine.

Let $\delta f(y)$ be of the special form $\epsilon h(y)$ where $h$ differs from zero only in the interval $(y — \mu, y + \mu)$ and be such that $\int h(x)dx = 1$. Because the functional derivative is supposed not to depend on the increment $\delta f$, the integral on the right of eqn (1.7) may be approximated by

$$
\begin{equation}  \tag{1.10}
\delta I[f, \delta f(y)]=\epsilon \frac{\delta I}{\delta f(y)}
\end{equation}
$$

Thus 

$$
\begin{equation}  \tag{1.11}
\frac{\delta I[f]}{\delta f(y)}=\displaystyle \lim_{{\epsilon  \to 0},{\mu  \to 0}}\frac{\delta I[f,\epsilon h]}{\epsilon}
\end{equation}
$$

Since $h(y)$ approximates the delta functional when used as an integral kernel, eqn (1.11) reduces to eqn (1.9).

Equation (1.9) leads to a formal expression that is often useful in evaluating functional derivatives expressed in the form of definite integrals

$$
\begin{equation}  \tag{1.12}
\frac{\delta f}{\delta f(y)}=\delta (x-x')
\end{equation}
$$

## Functional Representation of Problem Solution
### Variational (Functional) Derivatives

Before really diving into this section, lets give some examples of functionals:

1. **The Evaluation Functional**: $F[x] = x(t_0)$. This returns the value of the path $x$ at a specific time $t_0$.
2. **The Integral Functional**: $F[x] = \int_0^T g(x(t)) dt$. This is commonly used in calculating the payoff of Asian options.
3. **The Maximum Functional**: $F[x] = \sup_{t \in [0,T]} x(t)$. This is central to the pricing of lookback and barrier options.

### From Functionals to Path-Dependence: The Dupire Framework

While the functional derivatives defined in eqns (1.9) and (1.11) are standard in physics (e.g., in Field Theory), Bruno Dupire introduced a specific "non-anticipative" calculus tailored for finance. In this framework, we consider functionals $F_t(x_s: 0 \le s \le t)$ that depend on the path of a process up to the current time $t$.

To differentiate these functionals, Dupire defined two operators:

1.  **The Horizontal Derivative ($\Delta_t$):** This measures the sensitivity of the functional to the passage of time, keeping the path constant but extending its observation window.
    $$ \Delta_t F = \lim_{h \downarrow 0} \frac{F_{t+h}(x_{s \wedge t}) - F_t(x_s)}{h} $$
2.  **The Vertical Derivative ($\nabla_x$):** This measures the sensitivity to an instantaneous jump at the current end-point of the path.
    $$ \nabla_x F = \lim_{h \to 0} \frac{F_t(x_s + h \mathbf{1}_{s=t}) - F_t(x_s)}{h} $$

### The Functional Itô Formula

The "Magic" of Dupire's calculus is the Functional Itô Formula. For a path-dependent functional $F$, the change $dF$ along a path $X$ following $dX_t = \mu_t dt + \sigma_t dW_t$ is given by:

$$
\begin{equation} \tag{2.1}
dF_t = \left( \Delta_t F + \mu_t \nabla_x F + \frac{1}{2}\sigma_t^2 \nabla_{xx}^2 F \right) dt + \sigma_t \nabla_x F dW_t
\end{equation}
$$

This formula allows us to represent the price of path-dependent options (like Asians or Lookbacks) as solutions to **Functional Partial Differential Equations (FPDEs)**. In the risk-neutral measure, where the drift is $r$, the fair value of a path-dependent claim satisfies:

$$
\begin{equation} \tag{2.2}
\Delta_t F + r x_t \nabla_x F + \frac{1}{2}\sigma^2(t, x_t) x_t^2 \nabla_{xx}^2 F - rF = 0
\end{equation}
$$

This generalizes the Black-Scholes PDE from functions of $(t, S_t)$ to functionals of the entire history $S_{[0,t]}$.
