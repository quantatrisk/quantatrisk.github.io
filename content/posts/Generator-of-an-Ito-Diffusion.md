---
title: "The Generator of an Itô Diffusion"
subtitle: "A Gateway to Connecting Stochastic Differential Equations, Partial Differential Equations, and the Feynman-Kac Formula in Quantitative Finance"
date: 2026-01-07T00:00:00+05:30  # Auto-detected from filename if omitted
draft: false

# Primary categorization (1-3 max)
categories: 
  - "Stochastic Processes"
  - "Mathematical Finance"
  - "Partial Differential Equations"

# Specific tags (5-10, SEO + discoverability)
tags: 
  - "Itô Diffusion"
  - "Infinitesimal Generator"
  - "Feynman-Kac Formula"
  - "Kolmogorov Equations"
  - "Dynkin's Formula"
  - "SDEs"
  - "PDEs"
  - "Black-Scholes PDE"
  - "Risk-Neutral Pricing"
  - "Quantitative Finance"

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
description: "Master the infinitesimal generator of Itô diffusions—the bridge connecting SDEs, PDEs, and Feynman-Kac formula. From Kolmogorov equations to Black-Scholes PDEs and risk-neutral pricing."
keywords: ["Itô generator", "Feynman-Kac", "stochastic PDEs", "quant finance", "SDE PDE duality"]

# Optional: social sharing
twitter: "quantatrisk"
github: "quantatrisk/quantatrisk.github.io"

---

<div style="margin-bottom: 2rem;">
  <video style="width: 100%; border-radius: 8px;" controls playsinline>
    <source src="/videos/infinitesimal_generator.mp4" type="video/mp4">
    Your browser does not support the video tag.
  </video>
</div>

The modern mathematical foundation for understanding continuous-time phenomena in quantitative finance rests upon a remarkable trinity of interconnected theories: the theory of Itô diffusion processes, the theory of partial differential equations (PDEs), and probabilistic methods for solving PDEs. At the heart of this trinity lies a deceptively elegant mathematical object: the infinitesimal generator of a stochastic diffusion process. This introduction surveys the conceptual landscape, historical development, and practical significance of this powerful framework, which has transformed mathematical finance and continues to underpin pricing, risk management, and portfolio optimization across the industry.

The generator serves as the fundamental bridge connecting stochastic differential equations (SDEs) to PDEs and enables the celebrated Feynman-Kac formula—a result that allows one to solve deterministic PDEs through probabilistic simulation, and conversely, to compute expectations of random processes through deterministic methods. Understanding these interconnections is essential for anyone serious about quantitative finance, as they illuminate why certain financial models work the way they do and provide the mathematical machinery for designing and analyzing new models.

## 1. Understanding the Infinitesimal Generator

### 1.1 Historical Context and Foundational Development

The theory of stochastic differential equations and their relationship to partial differential equations has deep historical roots that deserve careful examination. The Japanese mathematician Kiyosi Itô pioneered this field in the 1940s, developing what is now universally called Itô calculus. [[1]](#ref-1) [[2]](#ref-2) [[3]](#ref-3)    Prior to Itô's work, Andrey Kolmogorov, had established in 1931 that continuous-time Markov processes could be characterized through differential equations, laying crucial groundwork for understanding the connection between stochastic processes and deterministic PDEs[[4]](#ref-4)[[5]](#ref-5)[[6]](#ref-6). Kolmogorov's fundamental insight was that there exist two classes of continuous-time processes: those with jumps (characterized by their jump rates) and those with continuous paths (characterized by diffusion and drift coefficients)[[6]](#ref-6). However, Itô's crucial innovation was to develop a calculus appropriate for stochastic processes, creating the theory of stochastic integration and the stochastic chain rule (Itô's lemma), which made practical computation with these equations feasible[[1]](#ref-1)[[2]](#ref-2).

The convergence of Feynman's path integral approach in physics and Kac's probability-theoretic methods occurred in 1947 at Cornell University, where they independently discovered that they were solving the same fundamental problem from different mathematical perspectives[[7]](#ref-7)[[8]](#ref-8). This serendipitous meeting led to the formulation of the Feynman-Kac formula, which established a rigorous, real-valued analogy to Feynman's earlier path integral methods. The formula proved that solutions to certain classes of parabolic PDEs could be represented as expectations of functionals of stochastic processes, a discovery that would eventually revolutionize mathematical finance.

### 1.2 The Infinitesimal Generator: Definition and Intuition

The infinitesimal generator of an Itô diffusion is perhaps best understood through its fundamental definition as a limit[[9]](#ref-9)[[10]](#ref-10)[[11]](#ref-11). For a continuous-time Markov process Xt defined on a probability space, the infinitesimal generator A is defined formally as

$$
\begin{equation}  \tag{1.1}
Af(x) = \lim_{t \downarrow 0} \frac{\mathbb{E}^x[f(X_t)] - f(x)}{t} 
\end{equation}
$$

where the expectation $\mathbb{E}^x$  is taken conditional on the initial state X0=x, and where f ranges over a suitable space of test functions[[9]](#ref-9)[[10]](#ref-10)[[11]](#ref-11). This definition captures the essential idea that the generator encodes information about the instantaneous rate of change of expectations for *smooth functions* applied to the process.

For an explicit Itô diffusion process satisfying the stochastic differential equation

$$
\begin{equation}  \tag{1.2}
dX_t = b(X_t)dt + \sigma(X_t)dB_t,
\end{equation}
$$

where $b:R^n→R^n$ is the drift coefficient, s \sigma:Rn→Rn×m is the diffusion coefficient, and $B_t$ is an $m$-dimensional Brownian motion, the generator takes the explicit form[[9]](#ref-9)[[12]](#ref-12)[[11]](#ref-11)

$$
\begin{equation}  \tag{1.3}
Af(x) = b(x) \cdot \nabla f(x) + \frac{1}{2}(\sigma(x)\sigma(x)^\top) \cdot \nabla \nabla f(x),
\end{equation}
$$

where $\nabla f$ denotes the gradient and $\nabla \nabla f(x)$ denotes the Hessian matrix. This is a **second-order partial differential operator**, and the coefficient $\sigma(x)\sigma(x)^\top$ is often denoted as the diffusion matrix or covariance structure of the process. The remarkable feature of this formula is that it shows how the local structure of a stochastic process—its drift and volatility—directly translates into a specific form of differential operator.

To build intuition, consider the simplest example: **standard Brownian motion** on $\mathrm{R}^n$, satisfying $dX_t=dB_t$ [[9]](#ref-9)[[12]](#ref-12). Here, $b=0$ and $\sigma=I$ (the identity matrix), so the generator becomes

$$
\begin{equation}  \tag{1.4}
Af(x) = \frac{1}{2}\Delta f(x),
\end{equation}
$$



where $\Delta$ is the Laplacian operator. This elegant result shows that Brownian motion, in its local behavior, is governed by the diffusion equation through its generator. The factor of $1/2$ emerges naturally from Itô's lemma and the quadratic variation of Brownian increments[[13]](#ref-13).

### 1.3 The Generator as an Infinitesimal Semigroup

To deepen understanding of the generator, we must introduce the theory of semigroups of operators, a fundamental framework in functional analysis[[14]](#ref-14)[[15]](#ref-15)[[16]](#ref-16)[[17]](#ref-17). A **semigroup** is a family of bounded linear operators  {$T_t$}$_{t≥0}$ acting on a Banach space (such as the space of continuous bounded functions) that satisfies the composition property

$$
\begin{equation}  \tag{1.5}
T_{t+s} = T_t \circ T_s \quad \text{and} \quad T_0 = I.
\end{equation}
$$

For Markov processes, the semigroup operators  $T_t$ often represent conditional expectations: $T_t f(x)=E_x [f(X_t)]$ [[14]](#ref-14)[[16]](#ref-16). The infinitesimal generator $A$ is then defined as the derivative of the semigroup at time zero

$$
\begin{equation}  \tag{1.6}
Af = \lim_{t \downarrow 0} \frac{T_t f - f}{t}.
\end{equation}
$$

This perspective reveals a profound mathematical structure: the **entire evolution of a Markov process through time can be encoded in a single operator, the generator**[[14]](#ref-14)[[15]](#ref-15)[[16]](#ref-16). One can formally write $T_t=e^{tA}$, meaning the semigroup is the exponential of the generator. This formula suggests that understanding the spectral properties of the generator—its eigenvalues, eigenfunctions, and other spectral decompositions—provides global information about the long-run behavior of the process[[18]](#ref-18)[[16]](#ref-16)[[19]](#ref-19).



## 2. Stochastic Differential Equations and Partial Differential Equations: A Fundamental Duality

### 2.1 The Kolmogorov Backward and Forward Equations

The connection between stochastic and deterministic worlds is made precise through two fundamental PDEs discovered by Kolmogorov[[4]](#ref-4)[[5]](#ref-5)[[6]](#ref-6). Consider an Itô diffusion $X_t$ with generator $A$, and define a function $u(t,x)=E^x [f(X_t)]$ representing the conditional expectation of some payoff function f under the process[[9]](#ref-9)[[4]](#ref-4). The **Kolmogorov backward equation** states that this function must satisfy

$$
\begin{align}  \tag{2.1}
\frac{\partial u}{\partial t}(t,x) = Au(t,x),  t > 0, \, x \in \mathbb{R}^n, \\
u(0,x) = f(x), x \in \mathbb{R}^n,
\end{align}
$$

This remarkable result shows that the generator, which characterizes the infinitesimal behavior of the stochastic process, also characterizes how expectations of the process evolve in time through a deterministic PDE[[9]](#ref-9)[[4]](#ref-4). The "backward" terminology arises because this equation is specified with an initial condition at time $t=0$ and evolves forward in time, though its derivation involves **conditioning on a future event**.

The **Kolmogorov forward equation**, also known as the **Fokker-Planck equation**, is the "adjoint" version of the backward equation[[9]](#ref-9)[[4]](#ref-4)[[6]](#ref-6). It describes how the probability density function $\rho(t,x)$ of $X_t$ evolves in time:

$$
\begin{align} \tag{2.2}
\frac{\partial \rho}{\partial t}(t,x) = A^{*}  
\rho(t,x), & t > 0, \, x \in \mathbb{R}^n, \\
\rho(0,x) = \rho_0(x), & x \in \mathbb{R}^n,
\end{align}
$$


where $A^{∗}$ is the Hermitian adjoint (or formal dual) of the generator $A$ with respect to the $L_2$ inner product[[9]](#ref-9)[[4]](#ref-4)[[6]](#ref-6). These two equations—backward and forward—represent complementary perspectives (in other words, they are equivalent) on the same underlying stochastic process: the backward equation describes expectations, while the forward equation describes probability densities.

Quoting from Gardiner [[77]](#ref-77):

> The forward equation gives more directly the values of measurable quantities as a function of the observed time $t$, and tends to used more commonly in applications. The backward equation finds most applications in the study of *fist passage time* or *exit problems*, [ref] in which we find the probability that a particle leaves a region in a given time.
>



### 2.2 Dynkin's Formula: Expected Values at Stopping Times

A crucial application of the generator is **Dynkin's formula**, named after Eugene Dynkin[[11]](#ref-11)[[20]](#ref-20)[[21]](#ref-21). This formula generalizes the Kolmogorov backward equation to random stopping times, rather than fixed times. Specifically, if $\tau$ is a stopping time (a random time that may depend on the path of the process but satisfies a non-anticipating property) with finite expectation $\mathbb{E}^x[\tau]< \inf$, and if $f$ is a suitably smooth function (typically $C^2$ with compact support), then

$$
\begin{equation} \tag{2.3}
\mathbb{E}^x[f(X_\tau)] = f(x) + \mathbb{E}^x\left[\int_0^\tau Af(X_s) ds\right].
\end{equation}
$$



This formula is profound: it relates the expected value of a function at a random time to the initial value plus the integrated action of the generator along the random path. Dynkin's formula is often described as a stochastic analogue of the fundamental theorem of calculus, for it captures how "accumulated infinitesimal changes" (represented by the generator integral) sum to the finite-time expectation[[11]](#ref-11)[[20]](#ref-20)[[21]](#ref-21). Practitioners use this formula extensively to compute expected hitting times, expected payoffs at exit times, and other quantities essential to risk analysis.



### 2.3 The Martingale Property and Its Applications



An important property of the generator is that it characterizes **martingales** associated with the process. Specifically, if f is a suitably smooth function lying in the domain of the generator, then the process



$$
\begin{equation} \tag{2.4}
M_t = f(X_t) - \int_0^t Af(X_s) ds
\end{equation}
$$



is a martingale with respect to the natural filtration generated by the process $X$ [[9]](#ref-9)[[22]](#ref-22)[[23]](#ref-23). This means that the conditional expectation of $M_t$ given past information equals the current value: $\mathbb{E}[M_t|F_s]=M_s$ for $t>s$. This martingale property is fundamental because martingales are the natural stochastic objects for pricing: in an arbitrage-free market, discounted asset prices are martingales under the risk-neutral measure, and the generator provides the mechanism for verifying and working with this property[[24]](#ref-24)[[25]](#ref-25).



## 3. The Feynman-Kac Formula: Unifying Stochastic and Deterministic Analysis



### 3.1 Statement and Derivation of the Feynman-Kac Formula



The **Feynman-Kac formula** is perhaps the most powerful result connecting SDEs, PDEs, and probabilistic computation[[7]](#ref-7)[[26]](#ref-26)[[27]](#ref-27)[[28]](#ref-28). In its classical form, it states that if $u(t,x)$ is a solution to the parabolic PDE



$$
\begin{equation} \tag{3.1}
\frac{\partial u}{\partial t}(t,x) + Au(t,x) - r(x,t)u(t,x) + f(x,t) = 0,
\end{equation}
$$



with terminal condition $u(T,x)=\psi(x)$, where $\mathcal{A}$ is the generator of an Itô diffusion $X_t$, then $u$ admits the probabilistic representation



$$
\begin{equation} \tag{3.2}
u(x,t) = \mathbb{E}\left[e^{-\int_t^T r(X_s,s)ds}\psi(X_T) + \int_t^T e^{-\int_t^\tau r(X_s,s)ds} f(X_\tau, \tau) d\tau \,\Big|\, X_t = x\right].
\end{equation}
$$



The derivation of this formula proceeds via Itô's lemma applied to a carefully constructed auxiliary process. Specifically, one defines $g(t,s) = e^{-\int_t^s r(X_r,r)dr}$ and constructs the process



$$
\begin{equation} \tag{3.3}
Y_s = g(t,s)u(X_s,s) + \int_t^s g(t,\tau)f(X_\tau,\tau)d\tau.
\end{equation}
$$



The crucial observation is that if $u$ satisfies the PDE above, then by Itô's lemma, $Y_s$ is a martingale (the drift term vanishes due to the PDE being satisfied). Consequently, $\mathbb{E}[Y_T|F_t]=Y_t$, which, upon taking expectations and rearranging, yields the desired probabilistic representation[[7]](#ref-7)[[26]](#ref-26).



### 3.2 Intuitive Interpretation: The Particle Metaphor



To build intuition for the Feynman-Kac formula, it is helpful to adopt a physical metaphor[[7]](#ref-7). Imagine a particle whose position at time $t$ is given by $X_t$, evolving according to the diffusion process governed by the SDE corresponding to generator $\mathcal{A}$ .
Suppose the particle incurs a "cost" at a rate of $f(X_s,s)$ at location $X_s$ at time $s$. Additionally, the particle "decays" or is "killed" at rate $r(X_s,s)$ — if the particle has decayed, all future costs become zero. Finally, upon survival to the terminal time $T$, the particle incurs a final cost (or receives a final payoff) of $\psi(X_T)$.



Under this interpretation, $u(x,t)$ represents the **expected total cost-to-go**, starting from position $x$ at time $t$. The factor $e^{-\int_t^s r(X_r,r)dr}$ represents the survival probability up to time $\tau$, the integral $\int_t^T e^{-\int_t^\tau r(X_s,s)ds} f(X_\tau,\tau)d\tau$ represents the expected accumulated cost, and $e^{-\int_t^T r(X_s,s)ds}\psi(X_T)$ represents the discounted terminal payoff [[7]](#ref-7)[[26]](#ref-26). This intuitive picture makes clear why the formula works: it is essentially a probabilistic accounting of expected costs and payoffs under the stochastic dynamics.



### 3.3 The Black-Scholes Equation as a Special Case



One of the most important applications of the Feynman-Kac formula in finance is to option pricing. Consider a European call option on a stock with price $S_t$ following geometric Brownian motion:



$$
\begin{equation} \tag{3.4}
dS_t = \mu S_t dt + \sigma S_t dW_t,
\end{equation}
$$



where $\mu$ is the (real-world) drift and $\sigma$ is the volatility. Under the risk-neutral measure $\mathbb{Q^*}$, the drift is replaced by the risk-free rate $r$, yielding



$$
\begin{equation} \tag{3.6}
dS_t = r S_t dt + \sigma S_t dW_t^*.
\end{equation}
$$



The generator of this process, acting on a function $V(t,S)$, is



$$
\begin{equation} \tag{3.7}
\mathcal{A}V = rS\frac{\partial V}{\partial S} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2}.
\end{equation}
$$



The price of a European call option with strike $K$ and expiry $T$ satisfies



$$
\begin{equation} \tag{3.8}
V(S,t) = \mathbb{E}^*\left[e^{-r(T-t)}(S_T - K)^+ | S_t = S\right],
\end{equation}
$$



which by the Feynman-Kac formula satisfies the **Black-Scholes PDE**:



$$
\begin{equation} \tag{3.9}
\frac{\partial V}{\partial t} + rS\frac{\partial V}{\partial S} + \frac{1}{2}\sigma^2 S^2 \frac{\partial^2 V}{\partial S^2} - rV = 0,
\end{equation}
$$


with boundary condition $V(S,T)=(S−K)^{+}$ [[29]](#ref-29)[[30]](#ref-30)[[31]](#ref-31)[[32]](#ref-32). This is the foundational equation for derivatives pricing, and the Feynman-Kac formula makes explicit that its solution is the discounted expectation of the payoff under the risk-neutral measure—the fundamental principle of arbitrage-free valuation.



## 4. Applications in Quantitative Finance



### 4.1 Pricing and Hedging



Beyond the canonical Black-Scholes formula, the framework of generators and the Feynman-Kac formula extends to a vast array of financial applications. **Path-dependent options** (such as Asian options, barrier options, and lookback options) have payoff functions that depend on the entire history of the underlying asset price, not merely its final value[[7]](#ref-7)[[27]](#ref-27). For such options, analytical solutions to the Black-Scholes PDE often do not exist in closed form, yet the Feynman-Kac formula provides a natural framework for numerical pricing via **Monte Carlo simulation**: one simulates the risk-neutral paths of the underlying assets and computes the discounted expected payoff. The generator encodes the risk-neutral dynamics that should be used in these simulations.



The **hedging** problem is intimately connected to the generator framework. The generator's connection to martingales ensures that if one constructs a self-financing replicating portfolio whose value evolves according to the Feynman-Kac representation, then that portfolio perfectly replicates the derivative's payoff[[29]](#ref-29)[[30]](#ref-30)[[31]](#ref-31). Moreover, the partial derivatives of the option value (the "Greeks": delta, gamma, vega, and rho) can be computed as partial derivatives of the PDE solution, revealing the sensitivity of the derivative to small changes in the underlying parameters—information essential for risk management.



### 4.2 Risk Management and Stress Testing



The generator formulation also illuminates **value-at-risk (VaR)** and **conditional value-at-risk (CVaR)** calculations, which are central to modern risk management[[27]](#ref-27). These quantities measure the risk of losses under different probability scenarios. Through the Feynman-Kac formula, one can relate these risk measures to solutions of PDEs, which can then be solved numerically to obtain VaR estimates. Furthermore, **stress testing**—evaluating losses under extreme but plausible market scenarios—is naturally formulated using the generator framework: stresses correspond to shifts in the drift and volatility coefficients of the generator, and one can study how the resulting PDE solutions (and hence expected future prices) change under these parameter perturbations.



### 4.3 Multi-Asset and Regime-Switching Models



Extensions of the generator framework to **multi-dimensional diffusions** and **regime-switching models** (where market regimes such as "bull" and "bear" markets switch stochastically) are both natural and powerful[[33]](#ref-33). In the multi-dimensional setting, the generator becomes a vector elliptic differential operator, and the Feynman-Kac formula applies with appropriate modifications. For regime-switching models, the generator must account for both the continuous diffusion dynamics within each regime and the discrete jump transitions between regimes, leading to a **hybrid generator** that combines the differential operator structure with an integral operator structure accounting for jumps[[33]](#ref-33).



### 4.4 Interest Rate Models and Term Structure



In **interest rate modeling**, the generator framework provides a unified language for studying affine term structure models (ATSMs), where bond prices are exponential-affine functions of the state variables. The generator of the state variable dynamics, combined with the Feynman-Kac formula, yields PDEs whose solutions give zero-coupon bond prices and bond option prices. This framework has been instrumental in understanding the properties of widely-used models such as the Hull-White, Vasicek, and CIR models[[7]](#ref-7)[[27]](#ref-27).



## Conclusion and Subsequent Topics



The infinitesimal generator of an Itô diffusion process serves as the fundamental bridge connecting the probabilistic world of stochastic differential equations to the deterministic world of partial differential equations. Through the elegant machinery of semigroup theory, the generator encodes the local dynamics of a continuous-time Markov process in a single differential operator. The Kolmogorov equations reveal how expectations and probability densities evolve under the generator's action. Dynkin's formula generalizes this to random stopping times, enabling computation of expected values at random times. Finally, the Feynman-Kac formula stands as the crowning achievement, establishing that solutions to parabolic PDEs can be represented as expectations of path-dependent functionals of stochastic processes, thereby uniting analysis and probability.



In quantitative finance, these tools are not merely of theoretical interest—they form the backbone of practical pricing, hedging, and risk management. The Black-Scholes equation, arguably the most famous PDE in finance, emerges naturally as a special case of the Feynman-Kac formula applied to European option pricing. Extensions to exotic options, multi-dimensional portfolios, regime-switching dynamics, and interest rate models all follow the same conceptual framework.



The subsequent sections of this article will develop these topics in depth. We shall provide rigorous mathematical treatments of the generator's definition and properties, derive the Kolmogorov equations with full care to technical details, prove Dynkin's formula under appropriate hypotheses, and establish the Feynman-Kac formula with complete proofs. We shall then explore applications to specific financial problems: the derivation of the Black-Scholes formula and its extensions, the pricing of exotic options via PDE methods and Monte Carlo simulation, the construction of interest rate models, and modern applications to credit risk and counterparty risk modeling. Throughout, the presentation will balance mathematical rigor with intuitive explanation, providing both the conceptual understanding necessary for mastery and the technical details required for implementation.


<div class="citation-list-block">

**References:**

<span id="ref-1">[1]</span> Kiyosi Itô - Wikipedia https://en.wikipedia.org/wiki/Kiyosi_It%C3%B4

<span id="ref-2">[2]</span> [PDF] On the Works of Kiyosi Itô and Stochastic Analysis http://homepage1.canvas.ne.jp/fuku1/JJM07.pdf

<span id="ref-3">[3]</span> [PDF] On Kiyosi Itô's Work and its Impact - Institut für Mathematik https://www.math.hu-berlin.de/~foellmer/papers/Gauss_Lecture.pdf

<span id="ref-4">[4]</span> Kolmogorov backward equations (diffusion) - Wikipedia https://en.wikipedia.org/wiki/Kolmogorov_backward_equations_(diffusion)

<span id="ref-5">[5]</span> Kolmogorov equations - Wikipedia https://en.wikipedia.org/wiki/Kolmogorov_equations_(continuous-time_Markov_chains)

<span id="ref-6">[6]</span> Kolmogorov equations https://en.wikipedia.org/wiki/Kolmogorov_equations

<span id="ref-7">[7]</span> Feynman–Kac formula - Wikipedia https://en.wikipedia.org/wiki/Feynman%E2%80%93Kac_formula

<span id="ref-8">[8]</span> Feynman-Kac formula: Group project Contents Introduction https://www.fuw.edu.pl/~marcnap/feynmankac.pdf

<span id="ref-9">[9]</span> Itô diffusion - Wikipedia https://en.wikipedia.org/wiki/It%C3%B4_diffusion

<span id="ref-10">[10]</span> Infinitesimal generator (stochastic processes) https://en.wikipedia.org/wiki/Infinitesimal_generator_(stochastic_processes)

<span id="ref-11">[11]</span> Dynkin's formula https://en.wikipedia.org/wiki/Dynkin's_formula

<span id="ref-12">[12]</span> 12 Itô Calculus and Controlled Diffusion Process http://maxim.ece.illinois.edu/teaching/spring19/notes/week12.pdf

<span id="ref-13">[13]</span> Itô's lemma - Wikipedia https://en.wikipedia.org/wiki/It%C3%B4's_lemma

<span id="ref-14">[14]</span> Generators of Markov Processes https://stat.cmu.edu/~cshalizi/754/notes/lecture-12.pdf

<span id="ref-15">[15]</span> Markov semigroups and groups of operators https://repository.lsu.edu/cgi/viewcontent.cgi?article=1017&context=cosa

<span id="ref-16">[16]</span> Markov Semigroups https://www.math.unipd.it/~daipra/didattica/Bologna12/MarkovSemgroups-12.pdf

<span id="ref-17">[17]</span> Markov Semigroups of Operators and Transition Functions http://article.scholarena.com/Markov-Semigroups-of-Operators-and-Transition-Functions.pdf

<span id="ref-18">[18]</span> Operator Methods for Continuous-Time Markov Processes https://www.princeton.edu/~yacine/AHS_operatormethods_handbook_10.pdf

<span id="ref-19">[19]</span> 6. Semigroups and Generators https://continuous-time-mcs.quantecon.org/generators.html

<span id="ref-20">[20]</span> optimal stopping time and its applications to economic ... http://math.uchicago.edu/~may/REU2020/REUPapers/Sanguanmoo.pdf

<span id="ref-21">[21]</span> Lecture 11: Some applications of the backward equation https://personal.math.ubc.ca/~holmescerfon/teaching/asa22/handout-Lecture11_2022.pdf

<span id="ref-22">[22]</span> Comparison of Markov processes by the martingale ... https://arxiv.org/pdf/1911.04274.pdf

<span id="ref-23">[23]</span> Generators, martingale problems, and stochastic equations https://mathweb.ucsd.edu/~williams/seminars/stoch/kurtzlecture1a19.pdf

<span id="ref-24">[24]</span> Martingale Approach to Pricing and Hedging https://personal.ntu.edu.sg/nprivault/MA5182/martingale-pricing-hedging.pdf

<span id="ref-25">[25]</span> Stochastic Calculus for Option Pricing https://arxiv.org/pdf/2408.05672.pdf

<span id="ref-26">[26]</span> Feynman-Kac formula for general time dependent stochastic ... - arXiv https://arxiv.org/html/2508.07793

<span id="ref-27">[27]</span> Feynman-Kac Equation in Finance (Guide) - Markets Portfolio https://marketsportfolio.com/feynman-kac-equation-finance/

<span id="ref-28">[28]</span> stochastic calculus applied to arbitrage-free options pricing https://math.uchicago.edu/~may/REU2019/REUPapers/Suresh.pdf

<span id="ref-29">[29]</span> Deriving the Black-Scholes Equation https://www.quantstart.com/articles/Deriving-the-Black-Scholes-Equation/

<span id="ref-30">[30]</span> Chapter 6 https://personal.ntu.edu.sg/nprivault/MA5182/black-scholes-pde.pdf

<span id="ref-31">[31]</span> stochastic calculus and black-scholes model https://math.uchicago.edu/~may/REU2017/REUPapers/Yoo.pdf

<span id="ref-32">[32]</span> Four Derivations of the Black Scholes PDE http://www.frouah.com/finance%20notes/Black%20Scholes%20PDE.pdf

<span id="ref-33">[33]</span> Integral equation characterization of the Feynman–Kac ... https://www.sciencedirect.com/science/article/pii/S2590037419300871

<span id="ref-34">[34]</span> [PDF] 8 Stochastic Differential Equations and their link with Partial ... https://web.dm.unipi.it/trevisan/teaching/Master/2016-sanminiato/slides/lezione8.pdf

<span id="ref-35">[35]</span> Stochastic Partial Differential Equations - Emergent Mind https://www.emergentmind.com/topics/stochastic-partial-differential-equations-spdes

<span id="ref-36">[36]</span> Essentials of Diffusion Processes and Itô's Lemma - George G. Pennacchi https://gpennacc.web.illinois.edu/ChPres08.pdf

<span id="ref-37">[37]</span> Stochastic Partial Differential Equations Lecture Notes https://page.math.tu-berlin.de/~scheutzow/SPDEmain.pdf

<span id="ref-38">[38]</span> The Feynman-Kac formula, partial differential equations and Brownian motion [QCT21/22, Seminar #12] https://www.youtube.com/watch?v=oXaUfqdQyas

<span id="ref-39">[39]</span> Stochastic partial differential equation https://en.wikipedia.org/wiki/Stochastic_partial_differential_equation

<span id="ref-40">[40]</span> A NOTE ON A FENYMAN-KAC-TYPE FORMULA https://projecteuclid.org/journals/electronic-communications-in-probability/volume-14/issue-none/A-Note-on-a-Feynman-Kac-Type-Formula/10.1214/ECP.v14-1468.pdf

<span id="ref-41">[41]</span> The Infinitesimal Generator of an Itô Diffusion https://www.youtube.com/watch?v=P7BBeFzCjVo

<span id="ref-42">[42]</span> [PDF] An Introduction to Stochastic PDEs - of Martin Hairer https://www.hairer.org/notes/SPDEs_Course.pdf

<span id="ref-43">[43]</span> [PDF] The Feynman-Kac formula - Arizona Math https://math.arizona.edu/~faris/talks/FKac.pdf

<span id="ref-44">[44]</span> 1 Ito Stochastic Differential Equations https://math.nyu.edu/~goodman/teaching/StochCalc/notes/drafts/l10.pdf

<span id="ref-45">[45]</span> Robert C. Dalang https://people.math.rochester.edu/faculty/cmlr/Preprints/Utah-Summer-School.pdf

<span id="ref-46">[46]</span> Week 6 1 Feynman Kac Formula https://math.nyu.edu/~goodman/teaching/StochCalc2020/week6/Week6.pdf

<span id="ref-47">[47]</span> An introduction to diffusion processes and Ito's stochastic ... http://www0.cs.ucl.ac.uk/staff/C.Archambeau/SDE_web/figs_files/ca07_RgIto_talk.pdf

<span id="ref-48">[48]</span> 21. Stochastic Differential Equations https://www.youtube.com/watch?v=qdbkvD4N-us

<span id="ref-49">[49]</span> The Feynman-Kac Formula for 1D Diffusion Process https://www.linkedin.com/pulse/feynman-kac-formula-1d-diffusion-process-julien-riposo-ph-d-cqf-dnwpe

<span id="ref-50">[50]</span> [PDF] Black-Scholes Option Pricing: PDEs, Probability, and MAtlAB https://personal.math.vt.edu/day/class_homepages/5726/BSPDEbk.pdf

<span id="ref-51">[51]</span> Equivalent Markov processes under gauge group | Phys. Rev. E https://link.aps.org/doi/10.1103/PhysRevE.92.052132

<span id="ref-52">[52]</span> Feller Processes and Semigroups https://www.stat.berkeley.edu/~pitman/s205s03/lecture27.pdf

<span id="ref-53">[53]</span> [PDF] The Black-Scholes Model https://www.columbia.edu/~mh2078/FoundationsFE/BlackScholes.pdf

<span id="ref-54">[54]</span> Infinitesimal generators (Chapter 37) - Stochastic Processes https://www.cambridge.org/core/books/stochastic-processes/infinitesimal-generators/32CCFD21A2E9DA1BA7DE6C348CAEA0F0

<span id="ref-55">[55]</span> Black–Scholes equation - Wikipedia https://en.wikipedia.org/wiki/Black%E2%80%93Scholes_equation

<span id="ref-56">[56]</span> Numerical Methods in the - Applications of Option Pricing https://uwaterloo.ca/computational-mathematics/sites/default/files/uploads/documents/lidan_chen_0.pdf

<span id="ref-57">[57]</span> An Introduction to Point Processes from a Martingale ... https://www.math.kth.se/matstat/fofu/PointProc3.pdf

<span id="ref-58">[58]</span> Kolmogorov Forward and Backward Equations as Adjoints https://www.youtube.com/watch?v=PaZ0L2hj7PE

<span id="ref-59">[59]</span> Diffusion Processes and their Sample Paths : Reprint of the 1974 Edition : Itô, Kiyosi, author : Free Download, Borrow, and Streaming : Internet Archive https://archive.org/details/diffusionprocess0000itki

<span id="ref-60">[60]</span> 1.5 Backward Kolmogorov equation - MIT https://www.mit.edu/~kardar/teaching/IITS/lectures/lec2/lec2.pdf

<span id="ref-61">[61]</span> Stochastic Calculus Notes, Lecture 7 1 The Ito integral with ... https://math.nyu.edu/~goodman/teaching/StochCalc2007/notes/l7.pdf

<span id="ref-62">[62]</span> Memoirs of My Research on Stochastic https://abelsymposium.no/symp2005/preprints/ito.pdf

<span id="ref-63">[63]</span> Connection between Martingale Problems and Markov ... https://tu-dresden.de/mn/math/stochastik/das-institut/beschaeftigte/franziska-kuehn/ressourcen/dateien/lecture-notes-bielefeld-kuehn.pdf

<span id="ref-64">[64]</span> Kolmogorov Backward Equation: Derivation and Interpretation https://www.youtube.com/watch?v=wrvHHNCRl7I

<span id="ref-65">[65]</span> Kiyosi Ito (1915 - 2008) - Biography - MacTutor History of Mathematics https://mathshistory.st-andrews.ac.uk/Biographies/Ito/

<span id="ref-66">[66]</span> Itô's stochastic calculus: Its surprising power for applications https://www.sciencedirect.com/science/article/pii/S0304414910000220

<span id="ref-67">[67]</span> Dynkin's formula - Prefetch https://prefetch.eu/know/concept/dynkins-formula/

<span id="ref-68">[68]</span> Itô Lemma - an overview https://www.sciencedirect.com/topics/engineering/ito-lemma

<span id="ref-69">[69]</span> Applied Stochastic Processes and Control for Jump- ... http://homepages.math.uic.edu/~hanson/pub/Slides/bk07fkebkefinal.pdf

<span id="ref-70">[70]</span> itˆo calculus and derivative pricing with risk-neutral measure http://math.uchicago.edu/~may/REU2012/REUPapers/Cytrynbaum.pdf

[71] Markov Processes, Semigroups and Generators. Fourth ... https://warwick.ac.uk/fac/sci/statistics/staff/academic-research/kolokoltsov/books/markhead.pdf

[72] Brownian motion, Ito's lemma, and the Black-Scholes ... https://www.linkedin.com/pulse/brownian-motion-itos-lemma-black-scholes-formula-part-chuan-shi-1d

[73] Semigroups, Boundary Value Problems and Markov ... http://ndl.ethernet.edu.et/bitstream/123456789/53803/1/Kazuaki%20Taira.pdf

[74] Long-time dynamics of stochastic differential equations https://idpoisson.fr/berglund/kesm_notes.pdf

[75] Itô's Calculus and the Derivation of the Black-Scholes ... https://papers.ssrn.com/sol3/Delivery.cfm/SSRN_ID1285245_code91227.pdf?abstractid=1022386&mirid=1&type=2

[76] Mokobodzki's intervals: An approach to Dynkin games ... https://www.sciencedirect.com/science/article/pii/S0304414925002303

<span id="ref-7">[7]</span> [77] Gardiner, Crispin W. "Handbook of stochastic methods for physics, chemistry and the natural sciences." Springer series in synergetics (1985).
</div>

