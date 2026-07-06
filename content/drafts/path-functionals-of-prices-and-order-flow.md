# Path-Functional Statistics and Statistical-Physics Response Theory for Indian Index Options

## A research memorandum for the Market Ecology / Maya programme

*Scope: (I) the statistics of signed increments, running extrema, and level crossings as instruments for strategy development and risk analytics; (II) linear and nonlinear response theory transplanted from statistical physics to market analysis; (III) the design of novel response functions specific to the Indian index options ecosystem (NIFTY, SENSEX and related complexes), with explicit hooks into the ICSE Ecology Monitor, the Options Ecology Platform's Master Objects (Edge, Crowding, Fragility, Capacity), and Maya's risk-concept ontology.*

---

## 0. Framing: why these two threads are one research programme

At first sight the two topics look independent: one is about path functionals of a single price trajectory (signs, maxima, minima, crossings), the other about how the market responds to perturbations. In fact they are two faces of the same object, and the point of this memo is to exploit that identity.

The modern microstructure literature — largely written by statistical physicists — established that the central empirical puzzle of price formation is a tension between two path statistics. The **signs** of order flow are extraordinarily persistent (a long-memory process with Hurst exponent near 0.7), yet **price increments** are nearly uncorrelated. The resolution is that the market's **response function** — the average price move conditional on a signed trade — decays in a finely tuned way that exactly compensates the persistence of signs. In other words, the statistics of signed increments *are* the input to the market's Green's function, and the diffusive (or non-diffusive) character of the price path is the output. You cannot study one rigorously without the other.

The second identity is specific to options markets. Dealer hedging converts the option book's Greek profile into a **state-dependent force field** acting on the underlying: net long-gamma dealers exert a restoring force toward high-open-interest strikes (pinning), net short-gamma dealers exert an anti-restoring force (gamma-wall breaks, cascades). The natural way to *measure* this force field without knowing dealer identities is precisely through the **path statistics** of Part I: level-crossing rates across strikes, escape times from high-gamma zones, the distribution of the time-of-day of extrema on expiry days, drawdown/drawup asymmetries near walls. And the natural way to *model* it is the **response formalism** of Part II: a **hedging kernel** that closes a feedback loop between spot moves and futures/options flow. The platform documents already gesture at this — "Gamma Wall Pin," "Gamma Wall Break," "Reflexive Malfunction," "replenishment half-life" are all names for **response functions** or their **fingerprints in crossing statistics**. This memo gives them a **rigorous measurement theory**.

---

# Part I — Signed increments, running extrema, and level-crossing statistics

*This section builds, from microstructure first principles, the three families of "path functional" statistics that recur throughout quantitative trading: the statistics of **signed increments** (the signs of returns and of order flow), of **running extrema** (maxima, minima, drawdowns, records, ranges), and of **level crossings** (how often and when a price visits a level, and how long it takes to first reach one). The aim is to keep the mathematics honest — real theorems, real formulas, real estimators — while continually asking what each object means and how a trader uses it. Read it front to back the first time; the later parts lean on the earlier ones.*

---

## 0. Orientation: what a "path functional" is, and the paradox that organizes everything

A price series is a path: a function $t \mapsto S_t$. Most of quantitative finance summarizes a path by a few numbers — mean return, variance, a Sharpe ratio — that throw away the *shape* of the path. Path functionals are the statistics that keep the shape. They answer questions that are invisible to mean and variance:

- **Signs.** Did the increments tend to keep their direction (persistence) or flip (anti-persistence)? How long are the runs?
- **Extrema.** How far did the path fall below its own running high (drawdown)? How often did it make a new high (records)? When during the day did the high occur?
- **Crossings.** How many times did the path pass through a given level? How long did it take to first get there?

Two facts make these worth an entire report. The first is that many of them have **exact, distribution-free answers** under natural null models — the arcsine laws, the reflection principle, Sparre Andersen universality, Rice's formula. That means you get a rigorous baseline *for free*, and the trading signal is very often the *deviation* of the empirical statistic from its null, not the statistic itself. The second is that these functionals are the natural language of the deepest empirical regularity in microstructure — the one we will call the **efficiency paradox** — and understanding that paradox is what turns the statistics from curiosities into strategy.

**The efficiency paradox, stated up front.** Decompose each return into a sign and a magnitude, $r_t = \varepsilon_t\ |r_t|$. Empirically, in essentially every liquid market:

1. The signs of **order flow** (was the aggressor a buyer or a seller?) are enormously **persistent** — their autocorrelation decays as a slow power law over hours or days.
2. The signs of **price returns** are almost **unpredictable** — their autocorrelation is tiny beyond a few ticks. This is **(weak-form) market efficiency**: you cannot forecast the next tick's direction from past directions well enough to beat costs.

These two facts seem contradictory: if the flow that pushes prices is predictable, why isn't the price? The resolution — that the market's **response to flow** decays in a precisely tuned way that cancels the flow's persistence — is the intellectual spine of modern microstructure, and it is stated entirely in terms of the **path functionals** of this report. We will arrive at it properly in Part 5; everything before is the equipment needed to understand it.

**Notation used throughout.** $S_t$ is a price (mid-quote unless stated). Log-returns are $r_t=\ln S_t-\ln S_{t-1}$. The sign of the $t$-th event is $\varepsilon_t \in \left\\{ -1, + 1 \right\\}$ — for returns, $\varepsilon_t\ = \text{sgn}(r_t)$; for order flow, $\varepsilon_t\ =+1$ for a buyer-initiated trade, $-1$ for a seller-initiated one. Running extrema are $M_t=\max_{s\le t}S_s$ and $m_t=\min_{s\le t}S_s$. The drawdown is $D_t=M_t-S_t$ (or in log terms $\ln M_t - \ln S_t$). The number of crossings of level $u$ in $[0,T]$ is $N_T(u)$; the first-passage time to $u$ is $\tau_u=\inf\{t:S_t\ge u\}$. Expectations under a null model are written $\mathbb{E}_0[\cdot]$.

--- 

# Part 1 — Microstructure foundations: where the path comes from

You cannot interpret the statistics of a price path without a model of how the path is *made*. This part builds the minimal machinery — the order book, the sign process, and three classical models (Roll, Glosten–Milgrom, Kyle) — that gives the later functionals their meaning. Skip it at your peril: most misuses of extrema and crossing statistics come from applying a continuous-diffusion null to a process that is, at the scale being measured, a discrete jump process shaped by a limit order book.

## 1.1 The limit order book and the two ways a price moves

A modern electronic market is a **limit order book (LOB)**: a queue of resting buy orders (bids) and sell orders (asks) at discrete price levels. The best bid $b_t$ and best ask $a_t$ define the mid $m_t=(a_t+b_t)/2$ and the spread $s_t=a_t-b_t$. Two event types move the mid:

- A **market order** (or marketable limit order) consumes liquidity: a buy market order lifts the ask, and if it clears the whole level, the ask steps up and the mid rises. These carry a sign $\varepsilon_t$ (buyer- or seller-initiated).
- A **limit order or cancellation** supplies or withdraws liquidity: a new bid inside the spread, or a cancellation of the best ask, moves the mid without any trade.

This two-channel structure is why "volume" and "price change" are only loosely coupled, and why signing trades correctly matters. The **Lee–Ready** and **quote rule** conventions sign a trade by comparing its price to the prevailing quotes: at or above the ask → buy ($+1$); at or below the bid → sell ($-1$); inside the spread → use the tick rule (compare to the last different price). Getting the sign right is the foundation of everything in Part 2; the tick rule alone is unreliable in fast markets where quotes move between trades.

**Intuition.** Think of the mid as a light object sitting on a bed of liquidity. Trades push it; limit orders and cancels reshape the bed under it. The "impact" of a trade is not a fixed constant — it depends on how deep the bed is right there, right now. That state-dependence is the seed of the nonlinear, transient impact we develop later.

## 1.2 The sign process $\varepsilon_t$ and why it is the primitive object

The single most structured, most studied object in high-frequency data is not the return — it is the **sign of order flow**. The reason is mechanical: large institutional orders (**metaorders**) are too big to execute at once without ruinous impact, so they are **split** into hundreds or thousands of child orders executed over minutes to days. Each child order carries the same sign as its parent. The result is that the sign sequence $\{\varepsilon_t\}$ inherits long stretches of the same sign — it is a **long-memory** process. We will quantify this in Part 2; for now, hold the picture that $\varepsilon_t$ is close to the *cause* and $r_t$ is close to the *effect*, mediated by the liquidity bed.

## 1.3 The Roll model: spread from the autocovariance of price changes

The simplest microstructure model with a testable path-functional prediction is **Roll (1984)**. Suppose the efficient (unobservable) price $p_t^*$ is a random walk, and the observed transaction price bounces between bid and ask around it: $p_t = p_t^* + c\,\varepsilon_t$, where $c$ is the effective half-spread and $\varepsilon_t=\pm1$ is an i.i.d. fair coin (the naive assumption — buyers and sellers arrive at random). Then the observed price *change* is $\Delta p_t = \Delta p_t^* + c(\varepsilon_t-\varepsilon_{t-1})$, and its first-order autocovariance is

$$\operatorname{Cov}(\Delta p_t,\Delta p_{t-1}) = -c^2.$$

The **bid–ask bounce induces negative serial correlation in transaction-price changes**, of a magnitude that reveals the spread:

$$\widehat{\text{spread}} = 2c = 2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}.$$

This is our first path functional with a trading payoff: a *negative* one-lag autocovariance of transaction prices is not a mean-reverting alpha — it is a measurement artifact of the spread, and any strategy that "trades the reversal" at that scale is really trying to earn the spread while paying the spread. It also tells you that **the sign process of transaction prices is anti-persistent at lag 1 purely from bounce**, which is why Part 2 insists on separating *order-flow* signs (persistent) from *transaction-price* signs (bounce-contaminated) and on working with mid-quotes when measuring genuine dynamics.

## 1.4 Glosten–Milgrom: the spread as adverse selection

Roll assumed random buyers and sellers. **Glosten–Milgrom (1985)** asks what happens when some traders are **informed**. A market maker posts quotes and cannot tell an informed trader from a noise trader. Every buy order is *slightly bad news* for the maker (maybe the buyer knows something), so the maker sets the ask above the expected value and the bid below it. The spread is then the maker's protection against **adverse selection**:

$$a_t = \mathbb{E}[V\mid \text{buy at }t],\qquad b_t=\mathbb{E}[V\mid \text{sell at }t],$$

where $V$ is the asset's true value. The key output is that **each signed trade moves the maker's belief**, so quotes ratchet in the direction of flow — this is the microscopic origin of *permanent* price impact and of the correlation between signed flow and future price. The path-functional consequence: the mid becomes a **martingale conditioned on public information**, but a *predictable* function of the (privately correlated) order flow. Hold this — it is half of the efficiency paradox. Order flow moves price permanently through belief-updating; that is why persistent flow does not create arbitrage even though it is predictable.

## 1.5 Kyle (1985): impact is linear, and $\lambda$ is the price of liquidity

**Kyle's model** is the one whose vocabulary the whole industry borrows. A single informed trader knows the terminal value $v\sim\mathcal N(p_0,\Sigma_0)$. Noise traders submit random order flow $u\sim\mathcal N(0,\sigma_u^2)$. A competitive market maker sees only the *total* order flow $y = x + u$ (informed demand $x$ plus noise $u$) and sets a price $p = p_0 + \lambda y$ that breaks even in expectation. Solving the resulting fixed point (the informed trader chooses $x=\beta(v-p_0)$ to maximize expected profit; the maker sets $\lambda$ to break even) gives the celebrated equilibrium

$$x = \beta(v-p_0),\quad \beta=\frac{\sigma_u}{\sqrt{\Sigma_0}},\qquad \lambda=\frac{1}{2}\frac{\sqrt{\Sigma_0}}{\sigma_u}.$$

Three lessons that echo through every later part:

1. **Impact is linear in order flow:** $\Delta p = \lambda\, y$. The slope $\lambda$ (**Kyle's lambda**) is *the price of liquidity* — how many rupees the mid moves per unit of net signed volume. Illiquid markets have large $\lambda$.
2. **$\lambda$ scales inversely with noise-trader volume $\sigma_u$.** More noise to hide behind → cheaper to trade → smaller impact. This is why impact is state-dependent and why we later measure it conditionally.
3. **The equilibrium price is a martingale** even though the informed trader is systematically pushing it — because the maker prices in the *average* informativeness of flow. Efficiency and predictable flow coexist. This is the same reconciliation as Glosten–Milgrom, in linear-Gaussian dress.

**Intuition for $\lambda$ as a spectral object later.** In the multi-asset world, $\lambda$ becomes a *matrix* (cross-impact): flow in one instrument moves the price of another. The eigenvectors of that matrix are the true liquidity factors of a complex. We will not need the matrix here, but keep the picture that "impact" is fundamentally a linear-response coefficient — that is the bridge to response theory.

## 1.6 Trade time, volume time, and subordination — the single most important practical idea

Real markets do not tick at a constant rate. Activity clusters: bursts of trades, then lulls. If you sample in **calendar time**, returns are heavy-tailed, heteroskedastic, and strongly intraday-seasonal, and *every* closed-form null in this report (which assumes i.i.d. or Gaussian increments) will be violated for reasons that have nothing to do with the effect you are hunting.

The fix is **subordination** (Clark 1973; Ané–Geman 2000). Model the price as a diffusion run under a random **business clock** $\Theta_t$ — the cumulative volume or cumulative number of trades:

$$S_t = B_{\Theta_t},\qquad \Theta_t=\text{traded volume up to }t.$$

Sampled in this clock — one observation per fixed volume bucket, or per fixed number of trades — returns become **much closer to i.i.d. Gaussian**: the heavy tails and clustering are largely absorbed into the time change. The trading lessons:

- Compute sign, extrema, and crossing statistics **in volume time or trade time**, not calendar time, whenever you intend to compare against a Brownian or i.i.d. null.
- Volatility is, to first order, **activity** — the number of trades or volume traded — a fact that reappears when we estimate volatility from crossing counts (Part 4).
- Intraday seasonality (the U-shaped volume/volatility of the day) is *mostly* a clock effect; subordination removes it more cleanly than ad hoc time-of-day dummies.

With the price-formation machinery and the correct clock in hand, we can now study the three path functionals.

---


# Part 2 — Statistics of signed increments

## 2.1 The sign–magnitude decomposition and why the factors are (almost) independent

Write every increment as $r_t=\varepsilon_t\,|r_t|$. Algebraically trivial; statistically profound, because the two factors carry nearly disjoint information and behave completely differently.

The **magnitude** $|r_t|$ (volatility) is *long-range correlated*: its autocorrelation decays as a slow power law,
$$\operatorname{Corr}(|r_t|,|r_{t+\ell}|)\sim \ell^{-\nu},\qquad \nu\in(0,1),$$
the mathematical face of **volatility clustering** — big moves follow big moves. It is also **multifractal**: the $q$-th moment $\mathbb{E}[|r|^q]$ scales in the sampling interval with an exponent that is nonlinear in $q$, so there is no single "volatility" but a spectrum of them. This is the domain of GARCH, of rough volatility, and of Part 3's extrema; here we set it aside and concentrate on the sign.

The **sign** $\varepsilon_t$ behaves in one of two sharply different ways depending on *what is signed*:

- **Signs of returns** are nearly uncorrelated beyond a few lags — the efficiency half of the paradox.
- **Signs of order flow** are strongly, persistently correlated — the long-memory half.

The near-independence of sign and magnitude is itself a useful modeling assumption: it justifies studying the *direction* process on its own, with volatility handled separately (e.g., by working in volume time, which flattens $|r_t|$).

## 2.2 Long memory of order flow: the power-law autocorrelation and the Hurst exponent

Empirically (Bouchaud–Gefen–Potters–Wyart 2004; Lillo–Farmer 2004) the **order-flow sign autocorrelation** is
$$C_\varepsilon(\ell)=\mathbb{E}[\varepsilon_t\,\varepsilon_{t+\ell}]\sim c\,\ell^{-\gamma},\qquad 0<\gamma<1.$$
Because $\gamma<1$, the sum $\sum_\ell C_\varepsilon(\ell)$ *diverges*: this is genuine **long memory**, not merely slow decay. The connection to the **Hurst exponent** comes from the variance of the cumulative sign (the "signed order flow" or net imbalance $\Sigma_n=\sum_{i=1}^n\varepsilon_i$):
$$\operatorname{Var}(\Sigma_n)\sim n^{2H},\qquad H=1-\tfrac{\gamma}{2}\in(0.5,1).$$
For an i.i.d. sign sequence $\gamma\to\infty$ effectively and $H=1/2$ (ordinary diffusion of the imbalance). Long-memory flow gives $H>1/2$: the imbalance is **super-diffusive**, i.e., order flow *trends*.

**Where the exponent comes from — the metaorder-splitting mechanism.** Suppose metaorders arrive with sizes $Q$ drawn from a heavy-tailed (Pareto) distribution $\Pr(Q>q)\sim q^{-\alpha}$, each executed as a run of same-signed child orders of length $\propto Q$. Then the sign autocorrelation inherits the tail: a classical result (Lillo–Mike–Farmer) gives
$$\gamma = \alpha-1.$$
So the persistence of order flow is a direct fingerprint of the **distribution of institutional order sizes**. This is not a metaphor — it is a testable, and repeatedly confirmed, quantitative link between an unobservable (the size distribution of hidden parent orders) and an observable (the decay of the sign autocorrelation).

**Trading reading.** A rolling estimate of $\gamma$ (equivalently $H$ of the flow imbalance) is a **regime descriptor for the participant mix**: splitting-dominated, institutional flow → small $\gamma$, long runs, strongly super-diffusive imbalance; noise-dominated flow → large $\gamma$, short runs. A sudden change in $\gamma$ signals a change in *who is trading*, often before it shows in price.

## 2.3 Persistence probability and the persistence exponent (Sparre Andersen)

A complementary, and beautifully universal, way to summarize a sign process is the **persistence probability**
$$Q(n)=\Pr[\text{no sign change in the first }n\text{ steps}] = \Pr[\varepsilon_1=\varepsilon_2=\dots=\varepsilon_n].$$
For a wide class of processes this decays as a power law $Q(n)\sim n^{-\theta}$, defining the **persistence exponent** $\theta$ — a number that is exquisitely sensitive to the underlying dynamics (Majumdar; Bray–Majumdar–Schehr, *Adv. Phys.* 2013).

The cornerstone result is the **Sparre Andersen theorem**: for *any* random walk with continuous, symmetric, i.i.d. increments — *regardless of the jump distribution, including arbitrarily heavy tails* — the probability that the walk stays positive (equivalently, does not change the sign of its increments in a related sense) for $n$ steps decays universally as
$$Q(n)\sim \frac{1}{\sqrt{\pi n}},\qquad \theta=\tfrac12.$$
The remarkable feature is **distribution-freeness**: fat tails do not change the exponent. This gives you a *calibration-free null* for questions like "how long should a spread stay on one side of fair value?" or "is this run of same-signed returns surprising?" — the answer under the efficient-market null is governed by $\theta=1/2$, with no parameters to fit.

Deviations are the signal. A measured $\theta<1/2$ on *return* signs (runs longer than Sparre Andersen predicts) is evidence of trending / positive feedback; $\theta>1/2$ (runs shorter) is evidence of mean reversion / pinning. On *order-flow* signs, the long memory of §2.2 already tells you $\theta$ will be anomalous; the point of the persistence framing is that it gives a second, independent estimator that does not require fitting a power law to a noisy autocorrelation.

## 2.4 The runs test and the run-length distribution

The most elementary sign statistic is the **run**: a maximal block of identical signs. Under the i.i.d. fair-coin null, with $n_+$ up-steps and $n_-$ down-steps ($n=n_++n_-$), the number of runs $R$ has the **Wald–Wolfowitz** mean and variance
$$\mathbb{E}_0[R]=\frac{2n_+n_-}{n}+1,\qquad \operatorname{Var}_0(R)=\frac{2n_+n_-(2n_+n_--n)}{n^2(n-1)},$$
and $R$ is asymptotically normal, giving a $z$-test. **Too few runs** → persistence (trending); **too many runs** → mean reversion (or bid–ask bounce, per Roll).

The finer object is the **run-length distribution**. Under i.i.d., run lengths are geometric, $\Pr(\text{length}=k)=p^{k-1}(1-p)$ with light (exponential) tails. Order splitting produces run lengths with **heavy, power-law tails** — this is the run-length face of §2.2's long memory, and segmenting the flow into inferred parent orders (via change-point or hidden-Markov methods on the sign-and-size sequence) is exactly **metaorder detection**: knowing a large parent order is mid-execution tells you the flow pressure will *persist*, which is both an alpha (impact anticipation) and a risk (you are trading against something that isn't done).

## 2.5 Order-flow imbalance: the workhorse short-horizon signal

The most robust short-horizon alpha in all of microstructure is the near-**linear** relation between mid-price changes and **order-flow imbalance (OFI)**. Cont–Kukanov–Stoikov (2014) showed that over horizons of seconds to minutes,
$$\Delta m_t \approx \beta\cdot \text{OFI}_t + \text{noise},\qquad \text{OFI}_t=\sum_{\text{events in window}}(\text{signed size at the best quotes}),$$
where the slope $\beta$ is the empirical Kyle's $\lambda$ and scales inversely with book depth. Crucially, OFI (which counts limit-order arrivals and cancellations at the touch, not just trades) explains mid-changes *far* better than trade imbalance alone, because both channels of §1.1 move the price.

**How it is used.**
- *Directional prediction.* A positive OFI over the last window predicts a positive mid-move over the horizon during which impact has not yet decayed. Enormous $R^2$ at the tick scale, shrinking rapidly with horizon and eaten by costs — so it is an **execution-timing** and **market-making** signal, not a standalone strategy.
- *Toxicity / adverse-selection filter.* A long same-signed run of OFI with *rising* trade sizes is the footprint of an informed or forced participant. This is when a market maker should widen or step away — the practical content of Glosten–Milgrom's adverse selection, and the microscopic definition of "toxic flow."
- *Crowding, measured cross-sectionally.* Compute signed flow per instrument/strike/participant-class and look at the **coherence** of the sign field — the leading eigenvalue of the cross-sectional correlation matrix of signed flows. In a balanced market it sits inside the Marchenko–Pastur bulk of a random matrix; its escape above the bulk edge is a quantitative "everyone is on the same side" alarm. This is how you turn a per-instrument sign statistic into a *systemic* crowding diagnostic.

## 2.6 Signed increments at low frequency: flows as labelled signs

The same machinery applies at daily frequency wherever signed, *labelled* flow data exist. Persistence of a flow series (e.g., a streak of net institutional selling), the cross-sign structure between two participant classes (one selling while another absorbs), and conditional continuation probabilities $\Pr[\varepsilon_{t+1}=+1\mid \varepsilon_t,\varepsilon_{t-1},\dots]$ estimated non-parametrically over short histories are all the low-frequency limit of this part. The continuation probability, in particular, is a *sufficient statistic* for a short-horizon directional model and is trivially interpretable: it is the empirically estimated transition kernel of the sign chain.

## 2.7 Summary of Part 2

Signs are the primitive. Order-flow signs are long-memory (exponent $\gamma$, Hurst $H=1-\gamma/2$), a fingerprint of metaorder-size tails ($\gamma=\alpha-1$). Persistence has a universal, distribution-free null ($\theta=1/2$, Sparre Andersen), so deviations are the trending/reverting signal. Runs tests and run-length tails detect structure and metaorders. Order-flow imbalance is the near-linear, high-$R^2$ short-horizon predictor of price, usable for execution timing, toxicity filtering, and — cross-sectionally — crowding detection. Return signs, by contrast, are a near-null baseline whose *deviations* flag regime change.

---

# Part 3 — Statistics of running extrema: maxima, minima, drawdowns, records, ranges

The running maximum $M_t=\max_{s\le t}S_s$ and its relatives are among the best-understood functionals in probability, and — this is the pedagogical gift of the subject — a surprising number of trading-relevant quantities have **closed forms** under Brownian or random-walk nulls. We build them up from the one principle that generates most of them.

## 3.1 The reflection principle and the distribution of the maximum

Let $W_t$ be a driftless Brownian motion with volatility $\sigma$, $W_0=0$. The **reflection principle** is the observation that for any path reaching level $u>0$ before time $T$, reflecting the post-hitting portion about $u$ gives an equally likely path ending at $2u-W_T$. Counting paths this way yields

$$\Pr[M_T\ge u]=2\,\Pr[W_T\ge u]=2\left(1-\Phi\!\left(\frac{u}{\sigma\sqrt T}\right)\right),\qquad u\ge 0,$$

and, because $\{M_T\ge u\}=\{\tau_u\le T\}$, this is simultaneously the **first-passage** statement: the probability of touching $u$ at some point in $[0,T]$ is twice the probability of being beyond $u$ at the *end*. The factor of two is the whole content of the reflection principle, and it is the reason "probability of touching a barrier" is not the same as "probability of finishing beyond it" — a distinction that separates correct from incorrect barrier-option intuition and correct from incorrect stop/target placement.

With drift $\mu$ (i.e., $X_t=\mu t+\sigma W_t$), the first-passage time to $a>0$ has the **inverse Gaussian** density

$$f_{\tau_a}(t)=\frac{a}{\sigma\sqrt{2\pi t^{3}}}\exp\!\left(-\frac{(a-\mu t)^2}{2\sigma^2 t}\right),\qquad t>0,$$

with the striking property that for $\mu\le 0$ the barrier is hit with probability $<1$ (the walk may drift away forever) while $\mathbb{E}[\tau_a]=a/\mu$ is finite only for $\mu>0$. This single density prices the "how long to reach my target / stop" question for any drifting-diffusion model of a position.

## 3.2 The three arcsine laws — the most counterintuitive result you will use

For driftless Brownian motion on $[0,T]$, define three times:

1. $T_+$ = the total time the path spends **above** its starting level;
2. $\theta_{\max}$ = the time at which the path attains its **maximum**;
3. $L$ = the time of the **last visit** to the starting level.

Lévy's theorem: **all three are identically distributed**, with the **arcsine density**

$$f(x)=\frac{1}{\pi\sqrt{x(1-x)}},\qquad 0<x<1\quad(\text{here }x=\text{time}/T),$$

whose CDF is $\frac{2}{\pi}\arcsin\sqrt{x}$. This density is **U-shaped** — it piles probability mass at the *endpoints* $0$ and $1$ and is smallest in the middle. The consequences are wildly anti-intuitive and directly relevant:

- A "fair" path most likely spends *almost all* of its time on **one side** of the origin — spending exactly half the time above and half below is the *least* likely outcome (and is precisely why naive intuitions about "balanced" intraday behaviour fail.) "Balanced" is not the typical case; lopsided is.
- The time of the maximum is most likely near the **beginning or the end** of the window, rarely in the middle.

**Trading reading.** The *empirical* distribution across days of the **time-of-intraday-high** (or of the fraction of the session spent above the open), benchmarked against the arcsine null (with corrections for drift and intraday volatility seasonality), is a clean, one-number-per-day **regime diagnostic**. Trending days exaggerate the endpoint mass (highs at the close); mean-reverting or pinned regimes push the distribution toward the middle and hump-shaped occupation times. On a fixed-expiry instrument, migration of the time-of-extremum toward the last hour of the expiry session is a quantitative signature of end-of-life instability. Majumdar and collaborators derived the drifted and Lévy-flight generalizations, so the null can be made realistic rather than merely Brownian.

## 3.3 Drawdown theory: expected maximum drawdown, CDaR, CED, and optimal leverage

The **drawdown** $D_t=M_t-S_t$ (in log or level terms) is the reflected process "distance below the running high." Its extreme, the **maximum drawdown** $\mathrm{MDD}_T=\max_{t\le T}D_t$, is the risk number practitioners feel most viscerally, and it has real theory.

**Expected MDD (Magdon-Ismail–Atiya–Pratap–Abu-Mostafa 2004).** For a Brownian motion with drift $\mu$ and volatility $\sigma$ over horizon $T$, the expected maximum drawdown has closed form in terms of a scaling function of the dimensionless quantity $\mu\sqrt T/\sigma$. The three regimes:

- **Zero drift** ($\mu=0$): $\mathbb{E}[\mathrm{MDD}_T]=\sqrt{\dfrac{\pi}{2}}\,\sigma\sqrt T\approx 1.2533\,\sigma\sqrt T$ — drawdown grows like $\sqrt T$, the same rate as volatility itself.
- **Positive drift**: $\mathbb{E}[\mathrm{MDD}_T]$ grows only **logarithmically** in $T$ — a profitable strategy's worst drawdown barely grows with time.
- **Negative drift**: it grows **linearly** in $T$ — a losing strategy's drawdown compounds.

The immediate practical lesson: **a drawdown limit that is constant in horizon is wrong**; the correct benchmark scales with $\sigma\sqrt T$ (zero-edge case), and observing MDD growing *faster* than $\sqrt T$ is statistical evidence your drift has turned negative — a principled kill-switch far better than a round-number stop.

**Coherent drawdown risk measures.** Two constructions make drawdown usable in optimization, where raw MDD (non-convex, unstable) fails:

- **Conditional Drawdown-at-Risk (CDaR)** (Chekhlov–Uryasev–Zabarankin 2005): the expected value of the worst $\alpha$-fraction of drawdowns over the path. It is **convex** in portfolio weights, hence directly optimizable, and it inherits the coherence properties of Conditional VaR applied to the drawdown process.
- **Conditional Expected Drawdown (CED)** (Goldberg–Mahmoud 2016): the tail mean of the *maximum* drawdown distribution, with a clean **risk-attribution** calculus (it is positively homogeneous, so Euler decomposition assigns each position its drawdown contribution).

**Optimal leverage under a drawdown floor (Grossman–Zhou 1993).** If you must never breach a fraction $\phi$ below your high-water mark, the growth-optimal policy is to hold a risky fraction **proportional to the distance to the floor**:
$$w_t \propto \frac{M_t-\phi M_t}{M_t}=\text{(cushion above the floor)}.$$
As you approach the floor you de-lever smoothly to zero; as you make new highs you re-lever. This is the theoretically correct template for de-risking rules and is strictly better than a **cliff-edge stop**, which (a) sells everything at the worst moment and (b), when many managers share it, **synchronizes** forced selling — a systemic hazard we return to in Part 5.

## 3.4 Record statistics: the calibration-free null for new highs

A **record** occurs at step $n$ when $S_n$ exceeds all previous values. The astonishing fact (Sparre Andersen universality again; Majumdar–Ziff) is that for *any* random walk with continuous, symmetric i.i.d. increments — **including arbitrarily heavy tails** — the record statistics are **distribution-free**:

$$\Pr[\text{record at step }n]=r_n=\binom{2n}{n}2^{-2n}\sim\frac{1}{\sqrt{\pi n}},\qquad \mathbb{E}[\#\text{records in }n\text{ steps}]\sim\sqrt{\frac{4n}{\pi}}.$$

Read that again: the expected number of new all-time highs after $n$ steps is $\sqrt{4n/\pi}$ **no matter how fat the tails are**. This is a null model that needs *zero calibration*.

**Trading reading.** "Buy on a new $n$-period high" is a **record-triggered** strategy, and its false-trigger rate under the efficient null is *exactly* the record rate above. So:

- Excess record frequency relative to $\sqrt{4n/\pi}$ is evidence of **drift/trend** (breakout food exists); a *deficit* is evidence of **confinement** — and confinement around a fixed level is *pinning*, giving a second, independent pinning statistic to corroborate the crossing-based one of Part 4. Wergen–Krug worked out the drift corrections for equities, giving the refined null when a modest drift is present.
- **Record ages** (times between successive records) add a second observable. Anomalously long inter-record gaps followed by clusters of records are the record-domain signature of **compression → breakout**.

## 3.5 Range statistics and range-based volatility estimators

The intraday **range** $R_T=M_T-m_T$ is a running-extrema functional with a known Brownian law (Feller). Its main use is **volatility estimation** far more efficient than close-to-close:

- **Parkinson (1980):** $\hat\sigma^2_P=\dfrac{1}{4\ln 2}\,\mathbb{E}\big[(\ln(H/L))^2\big]$ — uses the high–low range; about 5× the efficiency of close-to-close because a single day's range contains far more information about $\sigma$ than a single close-to-close return.
- **Garman–Klass (1980):** adds the open and close; more efficient still.
- **Rogers–Satchell (1991):** **drift-robust** (unbiased even when $\mu\neq0$).
- **Yang–Zhang (2000):** **drift- and overnight-robust** — combines overnight (close-to-open) and intraday components; the estimator of choice when gaps matter.

**Why a trader cares beyond efficiency.** Any **straddle-breakeven** analysis compares the implied straddle width to the *expected realized range*, so the correct realized leg is a Yang–Zhang-type estimator, not close-to-close vol — and computed in the relevant expiry-cycle clock. Moreover, the **range/volatility ratio** and its distribution against the Feller null detect *compressed-range* regimes (range small relative to close-to-close vol), which are precisely the coiled-spring states that precede breakouts — the range-based echo of the record-deficit signal of §3.4.

## 3.6 Drawdown outliers, "dragon kings," and cascade detection

Johansen–Sornette showed that the largest drawdowns of major indices are **outliers** relative to the distribution fitted to small and moderate drawdowns — they do not lie on the same curve. The interpretation: crashes are not merely the fat tail of daily returns but **transient bursts of dependence** (consecutive losses becoming correlated), a qualitatively different beast ("dragon kings," Sornette). The practical procedure:

1. Extract **$\epsilon$-drawdowns**: peak-to-trough moves that ignore counter-moves smaller than a threshold $\epsilon$ (tie $\epsilon$ to current realized vol).
2. Fit the **bulk** of the $\epsilon$-drawdown size distribution (a stretched exponential fits well).
3. Monitor the **tail residuals**: when moderate drawdowns begin deviating *upward* from the bulk fit, dependence is building — a statistically disciplined "cascade risk rising" indicator, distinct from any volatility measure.

### 3.7 Strategy applications

Breakout and trend systems are, formally, record-triggered strategies, and their entire calibration problem is a record-statistics problem: the false-trigger rate of "buy at a new $n$-day high" is *exactly* the universal record rate under the null, so the excess conditional performance after records — stratified by regime, by expiry phase, by dealer-gamma sign — is the cleanest way to measure whether breakout food exists (the Food Web's "directional" species). Trailing stops are running-max functionals: a $k\%$ trailing stop exits at the first passage of the drawdown process $D_t$ through $kM_t$, so the exit-time distribution, the probability of stopping out before a target, and the cost of the stop in expectation are all computable under drifted-Brownian or bootstrap nulls — turning stop placement from folklore into a first-passage optimization. Time-under-water (duration of the current drawdown spell) has known excursion-theory laws and is the correct statistical basis for manager de-allocation rules in the Manager State layer: a manager should be reviewed when the *duration* of the drawdown is improbable under their own historical vol, not merely when its depth crosses a round number.

### 3.8 Risk applications: synchronized stops and cascade probability

The platform documents identify "stop-loss synchronization" as a core fragility. Extrema statistics make this computable. If $k$ managers run trailing or fixed stops at distances $d_1,\dots,d_k$ from their respective high-water marks, and their P&Ls load on a common factor (short weekly gamma, say) with known betas, then the probability that $\geq m$ stops are hit within a window $\Delta$ is a *joint first-passage* problem for correlated reflected processes — analytically hard but trivially simulable, and the simulation needs exactly the inputs the platform already maintains: exposure overlap, stop distances, factor vol. The output — a cascade-trigger probability as a function of a spot move — is the quantitative core of Forced-Exit Cascade Risk, and coupling it to the hedging response kernel of Part III turns it into a genuine feedback simulation (a stop cluster releases flow, flow moves spot through the impact kernel, spot movement hits further stops). This is the Simulation Sandbox's most valuable experiment.


## 3.9 First-passage strategies: brackets, trailing stops, time-under-water

The first-passage time $\tau_u = \inf\{t: S_t \geq u\}$ of drifted Brownian motion is inverse-Gaussian distributed. Every practical use of extrema reduces to a **first-passage time** computation, and framing it that way turns folklore into optimization.

- **Bracket (take-profit / stop-loss) orders** are a *double-barrier* first-passage problem. For drifting Brownian motion with target $+a$ and stop $-b$, the classical **gambler's-ruin** solution gives the probability of hitting the target first,
$$\Pr[\text{TP before SL}]=\frac{1-e^{2\mu b/\sigma^2}}{e^{-2\mu a/\sigma^2}-e^{2\mu b/\sigma^2}},$$
(with the $\mu\to0$ limit $b/(a+b)$), plus closed forms for the expected exit time. Fat tails **shorten** passage times to distant barriers relative to the Gaussian null (a jump can leap the interior); volatility clustering makes passage times **bimodal** (fast exits in bursts, slow exits in calm). The disciplined approach: compute *empirical* exit statistics in volume time, compare to the subordinated-Gaussian null, and let the **discrepancy** be the signal — e.g., stop-hit hazard rising above null while target-hit hazard does not is a quantitative "adverse regime for brackets" flag.

- **Trailing stops** are functionals of the *drawdown* process: a $k\%$ trailing stop exits at the first passage of $D_t$ through $k M_t$. So the exit-time distribution, the probability of being stopped before a target, and the expected cost of the stop are all computable under drifted-Brownian or bootstrap nulls — stop placement becomes a first-passage optimization rather than a guess.

- **Time-under-water** — the *duration* of the current drawdown spell — has known excursion-theory laws and is the correct basis for manager/strategy de-allocation: review a strategy when the drawdown's **duration** is improbable under its own historical volatility, not merely when its **depth** crosses a round number. Depth and duration are different excursion functionals and carry different information.

## 3.10 Summary of Part 3

The reflection principle generates the maximum's distribution and first-passage laws (inverse Gaussian with drift). The arcsine laws show "balanced" paths are atypical and turn the time-of-extremum into a regime gauge. Drawdown theory gives the $\sqrt T$ / $\log T$ / linear-$T$ scaling of expected MDD, the coherent measures CDaR and CED, and the Grossman–Zhou proportional-de-leveraging rule that dominates cliff stops. Record statistics give a **calibration-free** null for new highs ($\sqrt{4n/\pi}$), whose excess/deficit reads trend/pinning. Range estimators (Parkinson → Yang–Zhang) are the efficient, drift/gap-robust volatility inputs to breakeven analysis, and compressed ranges flag coiled springs. Drawdown-outlier analysis detects cascades. And every stop/target/trailing rule is, correctly seen, a first-passage optimization.

---

## 4.1 Rice's formula: the crossing rate of a smooth process

For a **smooth, stationary** process $X_t$ with joint density $p(x,\dot x)$ of the level and its time-derivative, the mean rate of **up-crossings** of level $u$ is **Rice's formula**:
$$\nu^+(u)=\int_0^\infty \dot x\,p(u,\dot x)\,d\dot x.$$
For a stationary **Gaussian** process with variance $\sigma^2$ and derivative-variance $\sigma_{\dot X}^2$, this evaluates to the clean form
$$\nu^+(u)=\frac{1}{2\pi}\frac{\sigma_{\dot X}}{\sigma}\,\exp\!\left(-\frac{u^2}{2\sigma^2}\right).$$
Two readings. First, the **zero-crossing rate** ($u=0$) is $\frac{1}{2\pi}\sigma_{\dot X}/\sigma$ — a ratio of spectral moments, i.e., a measure of the process's characteristic frequency; more high-frequency energy → more crossings. Second, the crossing rate **falls off as a Gaussian in the level** $u$: far-from-the-money levels are crossed exponentially rarely, which is exactly the shape you would want for a "how often does spot visit this strike" profile.

## 4.2 Diffusions are not smooth: local time and the Tanaka formula

Rice's formula needs a differentiable path. Brownian motion is **nowhere differentiable**, and in fact crosses *every* level it reaches **infinitely often** in any interval — the naive crossing count is infinite. The right object is not a count but a **density**: the **local time** $L_t^u$, the "amount of time" (measured in units of quadratic variation) the process spends at level $u$. It is defined as the occupation density
$$\int_0^t f(S_s)\,d\langle S\rangle_s=\int_{-\infty}^{\infty} f(u)\,L_t^u\,du\quad\text{for all test functions }f,$$
and it enters the **Tanaka formula**, the "absolute-value Itô rule":
$$|S_t-u|=|S_0-u|+\int_0^t \operatorname{sgn}(S_s-u)\,dS_s+L_t^u.$$
Local time is the rigorous version of "how much did the path hang around level $u$." Two practical handles on it:

- **Discretized crossings estimate local time.** If you discretize the level into a band $[u,u+\delta]$, the number of crossings $N_t(u,\delta)$ satisfies $\delta\,N_t(u,\delta)\to L_t^u$ as $\delta\to0$. So an *empirical* crossing count of a thin band is a consistent estimator of local time — the bridge from the (finite, observable) world of ticks to the (idealized) continuum object.
- **Crossings estimate volatility.** For a diffusion sampled on a fixed price grid, the number of grid crossings over $[0,T]$ scales with $\sigma$: more volatile → more crossings of any fixed lattice. This yields **crossing-based volatility estimators** that are robust to certain data pathologies. The caveat is severe: **bid–ask bounce inflates crossing counts** at fine scales, so one must use mid-quotes, pre-average, or count in trade/volume time.

## 4.3 The level profile of crossings: an occupation scan of the path

The single most useful *derived* object is the **crossing/occupation profile**: $\hat\nu(u)$ or $\hat L_T^u$ computed across a *grid of levels* $u$ is an occupation/energy-landscape scan of the price path: under the diffusive null it is smooth and unimodal around the current price, and decaying (roughly Gaussian-in-$u$ per Rice) away from it. **Anomalies in the profile are the signal:**  For example, dealer pinning creates a characteristic anomaly at high-OI strikes — elevated local time (spot lingers at the strike) with suppressed net escapes. 

- A local **spike in occupation with suppressed net escape** at a particular level is the fingerprint of a **magnet/pin** — the path lingers there and struggles to leave. In options markets this happens at high-open-interest strikes (dealer hedging creates an effective restoring force); the occupation profile *measures* the pin without knowing who is hedging.
- The econophysics literature developed exactly this under the names **level-crossing analysis** and (its horizon-dual) **inverse statistics**, applied to equity indices; the innovation available to a desk with granular data is to compute these **conditionally** — strike-relative, expiry-phase-conditional, regime-conditional — which no academic dataset can match.

**Crossings define events, and events define conditioning.** The deepest practical point: a crossing is a **natural event trigger**. "Spot crossed level $u$ from below, with a persistent same-signed order-flow imbalance behind it" is a level-crossing event *dressed with the sign statistics of Part 2* — precisely the kind of well-defined, measurable trigger a systematic strategy needs, and far cleaner than a threshold on a smoothed indicator.

## 4.4 First-passage revisited, and two pieces of physics worth stealing

Part 3 gave the first-passage laws; here are two extensions from statistical physics that map cleanly onto trading policy.

**Sparre Andersen for one-sided persistence.** As noted in §2.3, the survival probability (no crossing of a reference level) of a symmetric random walk decays as $n^{-1/2}$ *universally*. Applied to a **spread or basis** relative to fair value, this gives a parameter-free null for "how long should a dislocation persist on one side?" — the deviation from $n^{-1/2}$ is the tradeable signal for relative-value entries and for sizing conviction that a dislocation is structural rather than noise.

**Stochastic resetting (Evans–Majumdar 2011).** A diffusing particle that is intermittently **reset** to its starting point at rate $r$ has a *finite* mean first-passage time to a target, with a **nontrivial optimal reset rate** $r^\ast$ that minimizes expected time-to-target — whereas a pure diffusion has infinite mean first-passage in some settings. The trading translation: model "flatten and re-enter at a reference price" as resetting. The theory then says there is an **optimal, nonzero re-entry aggressiveness** after a stop-out, and it tells you how $r^\ast$ scales with volatility and target distance. This is a rigorous foundation for re-engagement rules that would otherwise be pure heuristic.

**Kramers escape as the pinning mechanism.** A particle in a potential well of barrier height $\Delta V$ subject to noise of intensity $D$ escapes at the **Arrhenius rate** $\sim e^{-\Delta V/D}$. A restoring force toward a level (buy below it, sell above it) *is* an effective potential well for the price; the well depth is set by the strength of the restoring flow, the noise $D$ by ambient volatility. **Pinning is residence in the well; a breakout is barrier escape.** The falsifiable prediction: residence times near a magnet level should be **exponentially distributed** with the Kramers rate implied by the measured restoring strength. Testing that (are escape times exponential with the predicted rate?) turns "pinning" from a story into a checkable hypothesis.

## 4.5 Inverse statistics and the optimal investment horizon

Flip the usual question. Instead of "given horizon $T$, what return distribution?", ask "given a target return level $\rho$, what is the distribution of the **first time** to achieve it?" This is the **inverse statistics** program (Simonsen–Jensen–Johansen 2002). The distribution of the first-passage time to a fixed return level has a well-defined **mode** — the **optimal investment horizon** for that target — and the mode grows with the target in a characteristic (roughly quadratic, diffusion-like) way.

The sharpest finding is the **gain–loss asymmetry**: for equity indices, the optimal horizon to achieve a **loss** of a given magnitude is *shorter* than the horizon to achieve a **gain** of the same magnitude. Drops happen faster than rises. This asymmetry is invisible to variance and to the return distribution at a fixed horizon; it lives entirely in the first-passage (crossing) statistics, and it is directly relevant to sizing the horizon of trend and mean-reversion bets and to the asymmetry of stop versus target placement.

## 4.6 Summary of Part 4

Rice's formula gives the crossing rate of smooth processes (Gaussian-in-level, spectral-ratio prefactor). Diffusions need **local time** instead of crossing counts; discretized band-crossings estimate local time, and grid-crossings estimate volatility (mind the bounce). The **crossing/occupation profile across levels** is an occupation scan whose anomalies reveal magnets/pins, and crossings supply clean **event triggers** when dressed with Part 2's sign context. First-passage physics contributes a parameter-free persistence null (Sparre Andersen), an optimal re-entry policy (stochastic resetting), and a falsifiable pinning mechanism (Kramers escape). Inverse statistics turn the return target into a horizon and reveal the gain–loss asymmetry that fixed-horizon statistics miss.

---

# Part 5 — Synthesis: the efficiency paradox resolved, and a working pipeline

We now have the three functionals. This part shows how they fit together — first conceptually (the paradox of Part 0), then operationally (a pipeline and its pitfalls).

## 5.1 The paradox resolved: impact as a decaying response to persistent flow

Recall the tension: order-flow signs are long-memory ($C_\varepsilon(\ell)\sim\ell^{-\gamma}$, $\gamma<1$, so the flow imbalance is super-diffusive with $H=1-\gamma/2>1/2$), yet prices are (nearly) a martingale. If each signed trade had a **permanent** impact $\lambda$ (à la Kyle), then the price would be $p_t\approx \lambda\sum_{s\le t}\varepsilon_s = \lambda\,\Sigma_t$, and its variance would grow like $\operatorname{Var}(\Sigma_t)\sim t^{2H}$ with $H>1/2$ — **super-diffusive, strongly trending prices**. That is not what we observe. Something must cancel the persistence.

The resolution (Bouchaud–Gefen–Potters–Wyart) is that impact is **transient**: the price is a *linear response* to past signed flow through a decaying kernel (a **propagator** / Green's function) $G$,
$$p_t=\sum_{s<t}G(t-s)\,\varepsilon_s + \text{noise}.$$
The empirical **response function** $\mathcal R(\ell)=\mathbb E[(p_{t+\ell}-p_t)\varepsilon_t]$ is the average price move a lag $\ell$ after a signed trade. For prices to be **diffusive** despite persistent flow, the propagator must decay as $G(\ell)\sim \ell^{-\beta}$ with the **critical balance**
$$\boxed{\;\beta=\frac{1-\gamma}{2}\;}$$
so that the trending tendency from persistent flow is *exactly* offset by the fading of old impact. Markets self-organize onto this critical line. Notice what this says: **the sign statistics of Part 2 are the *input* to the impact response, and the (near-)martingale property is the *output*** — the two halves of the paradox are related by the propagator, and the relationship is a statement about path functionals. The residual $\xi=\hat\beta-(1-\hat\gamma)/2$ is a one-number regime gauge: $\xi>0$ (impact decays too fast for the flow's persistence) means locally mean-reverting/liquidity-rich; $\xi<0$ means too-permanent impact, trending, fragile.

**The nonlinear companion — the square-root law.** For an entire **metaorder** of size $Q$ (not a single child order), impact is not linear but concave:
$$\Delta P\approx Y\,\sigma\,\sqrt{Q/V},\qquad Y=O(1),$$
where $\sigma$ is volatility and $V$ is market volume over the execution window. This **square-root law** is one of the most universal regularities in finance (across assets, eras, geographies). Its significance for this report: it is a *statement about the extremum of an execution path* (peak impact) as a function of the flow injected, and inverting it gives the **capacity** of a strategy — the maximum $Q$ deployable for a stated impact-cost budget. It connects Part 2 (flow) to Part 3 (the extremum reached) with a single calibrated constant $Y$.

## 5.2 How the three functionals interlock in practice

The functionals are not independent tools; they are three views of one path, and they **corroborate** each other:

- **Trend / mean-reversion regime** shows up as: small sign-persistence exponent $\theta$ or excess runs the wrong way (Part 2); *excess* records vs $\sqrt{4n/\pi}$ (Part 3); and $\xi<0$ in the propagator balance (Part 5.1). Three independent estimators of the same regime; agreement is confidence, disagreement is a data or model problem.
- **Pinning / confinement** shows up as: a *deficit* of records and compressed range (Part 3); an occupation spike with suppressed escape and exponential (Kramers) residence times at the level (Part 4); and short runs / $\theta>1/2$ on returns (Part 2). Again, three lenses on one phenomenon.
- **Crowding / fragility** shows up as: sign-field coherence (leading eigenvalue escaping the random-matrix bulk, Part 2); drawdown-outlier residuals departing the bulk fit (Part 3); and synchronized first-passage of many stops (Part 3, §5.3 below).

## 5.3 A worked hazard: synchronized stops as a joint first-passage cascade

Bring Part 3's first-passage and Part 2's flow together to make a fragility computation. Suppose $k$ strategies run trailing or fixed stops at distances $d_1,\dots,d_k$ from their high-water marks, and their P&Ls load on a common factor with betas $\beta_i$. The probability that $\ge m$ stops trigger within a window $\Delta$ is a **joint first-passage problem for correlated reflected processes** — analytically hard, trivially **simulable**. And it *feeds back*: a cluster of stops releases same-signed flow, which moves the price through the impact kernel of §5.1, which hits *more* stops. Coupling the joint-first-passage simulation to the propagator turns a static risk number into a genuine **cascade simulation**, and it is the quantitative core of "forced-exit" risk. The Grossman–Zhou proportional-de-leveraging rule of §3.3 is the mitigant precisely because it spreads the forced flow through time and desynchronizes the exits, lowering the joint-hit probability that drives the cascade.

## 5.4 A concrete estimation pipeline

Putting it together, a defensible research pipeline for these statistics:

1. **Clean and sign.** Reconstruct mid-quotes; sign trades by the quote rule with a tick-rule fallback (Part 1). Build the order-flow-imbalance series (Part 2). Gate everything by a data-reliability flag — crossing and record counts are exquisitely sensitive to stale quotes and dropped ticks.
2. **Subordinate.** Move to **volume time or trade time** before applying any closed-form null (Part 1.6). This is what makes the arcsine, reflection, Sparre Andersen, and Rice nulls trustworthy.
3. **Compute the functionals as *relative* statistics.** For each — sign autocorrelation/persistence, run lengths, records, drawdowns, occupation profile, first-passage/exit times — report the **observed minus null**, never the raw count. The null is arcsine/reflection/Sparre-Andersen/Feller where an exact result exists, and a **block bootstrap or model-simulated** null (rough-vol or Hawkes) where clustering must be preserved while the hypothesized structure is destroyed.
4. **Corroborate across functionals** (§5.2) before acting; a regime call supported by only one lens is a hypothesis, not a signal.
5. **Control error rates.** With many levels, many statistics, and many regimes, multiple-testing is severe. Use false-discovery control (Benjamini–Hochberg at minimum) for diagnostics and the **deflated-Sharpe / White reality-check / stationary-bootstrap** machinery for any strategy claim.
6. **Evaluate out-of-sample and track record.** Log every signal with its horizon and outcome; retire functionals whose warnings do not discriminate, regardless of theoretical pedigree.

## 5.5 Pitfalls, stated bluntly

- **Closed-form nulls assume i.i.d./Markov/Gaussian increments.** Real increments are heteroskedastic, heavy-tailed, seasonal, and regime-switching. *Subordinate first*, and where subordination is insufficient, use bootstrap/simulated nulls. A "significant" arcsine or record deviation computed in calendar time is very often just volatility seasonality.
- **Microstructure noise inflates fine-scale crossing and record counts.** Bid–ask bounce alone manufactures crossings (Roll, Part 1.3). Use mid-quotes, pre-averaging, or tick/volume-time sampling.
- **Transaction-price reversals are not alpha.** The negative lag-1 autocovariance of transaction prices is the spread (Roll). Measure dynamics on mid-quotes.
- **Correlation is not response.** A propagator or impact kernel estimated by regressing price on flow is *confounded* by the flow's own autocorrelation (the "dressed" vs "bare" kernel distinction) and by common drivers. Deconvolve, and where possible identify with exogenous kicks (scheduled news, auctions) or randomized execution — the only true controlled experiment a trader has.
- **Regimes break.** Market structure changes (rule changes, participant-mix shifts) create distinct populations. Pool across a structural break and you manufacture spurious statistics. Use explicit regime dummies at known effective dates.
- **Multiple testing is the default failure mode.** Hundreds of levels times dozens of statistics guarantees false discoveries without FDR/reality-check discipline.

## 5.6 What to take away

The three families are one subject. **Signed increments** are the primitive (order flow is long-memory; its exponent fingerprints the metaorder-size distribution; persistence has a distribution-free null). **Running extrema** convert a path into risk and regime numbers with exact scaling laws (reflection, arcsine, $\sqrt T$/$\log T$ drawdown scaling, calibration-free record rates, efficient range volatility). **Level crossings** measure how a path visits levels — local time and occupation profiles reveal magnets and supply event triggers, first-passage physics gives optimal re-entry and a falsifiable pinning test, inverse statistics reveal the gain–loss asymmetry. They meet in the **propagator**, where the persistence of flow (Part 2) and the transience of impact conspire to make prices diffusive (the efficiency paradox), and in the **cascade**, where synchronized first-passage (Part 3) feeds back through impact to threaten the system. Used as *relative* statistics against honest nulls, in the right clock, with multiple-testing discipline and cross-functional corroboration, they are among the most rigorous and least crowded tools in the systematic trader's kit.

---

## Appendix: formula quick-reference

| Object | Result | Where |
|---|---|---|
| Roll spread | $\widehat{s}=2\sqrt{-\operatorname{Cov}(\Delta p_t,\Delta p_{t-1})}$ | §1.3 |
| Kyle impact | $\Delta p=\lambda y,\ \lambda=\tfrac12\sqrt{\Sigma_0}/\sigma_u$ | §1.5 |
| Flow sign autocorr. | $C_\varepsilon(\ell)\sim\ell^{-\gamma}$, $H=1-\gamma/2$, $\gamma=\alpha-1$ | §2.2 |
| Persistence (Sparre Andersen) | $Q(n)\sim n^{-1/2}$, distribution-free | §2.3 |
| Runs test | $\mathbb E_0[R]=\tfrac{2n_+n_-}{n}+1$ | §2.4 |
| Order-flow imbalance | $\Delta m\approx\beta\,\text{OFI}$ | §2.5 |
| Max of BM (reflection) | $\Pr[M_T\ge u]=2(1-\Phi(u/\sigma\sqrt T))$ | §3.1 |
| First passage (drift) | inverse Gaussian, $\mathbb E[\tau_a]=a/\mu$ ($\mu>0$) | §3.1 |
| Arcsine density | $f(x)=1/(\pi\sqrt{x(1-x)})$ | §3.2 |
| Expected MDD ($\mu=0$) | $\sqrt{\pi/2}\,\sigma\sqrt T\approx1.2533\,\sigma\sqrt T$ | §3.3 |
| Grossman–Zhou | $w_t\propto$ distance to drawdown floor | §3.3 |
| Record rate (universal) | $r_n\sim1/\sqrt{\pi n}$, $\mathbb E[\#]\sim\sqrt{4n/\pi}$ | §3.4 |
| Parkinson vol | $\hat\sigma^2=\tfrac{1}{4\ln2}\mathbb E[(\ln(H/L))^2]$ | §3.5 |
| Double-barrier hit prob. | $(1-e^{2\mu b/\sigma^2})/(e^{-2\mu a/\sigma^2}-e^{2\mu b/\sigma^2})$ | §3.7 |
| Rice crossing rate | $\nu^+(u)=\tfrac{1}{2\pi}\tfrac{\sigma_{\dot X}}{\sigma}e^{-u^2/2\sigma^2}$ | §4.1 |
| Tanaka / local time | $\|S_t-u\|=\|S_0-u\|+\int\operatorname{sgn}\,dS+L_t^u$ | §4.2 |
| Local time from crossings | $\delta\,N_t(u,\delta)\to L_t^u$ | §4.2 |
| Kramers escape | rate $\sim e^{-\Delta V/D}$ | §4.4 |
| Propagator critical balance | $\beta=(1-\gamma)/2$ | §5.1 |
| Square-root impact | $\Delta P\approx Y\sigma\sqrt{Q/V}$ | §5.1 |

## Selected reading

Bouchaud, Bonart, Donier, Gould, *Trades, Quotes and Prices: Financial Markets Under the Microscope* (CUP, 2018) — the definitive synthesis. Bouchaud, Gefen, Potters, Wyart, "Fluctuations and response in financial markets" (2004). Lillo, Farmer, "The long memory of the efficient market" (2004); Lillo, Mike, Farmer, order-splitting theory of $\gamma=\alpha-1$ (2005). Kyle, "Continuous auctions and insider trading," *Econometrica* (1985); Glosten, Milgrom (1985); Roll (1984). Cont, Kukanov, Stoikov, "The price impact of order book events" (2014). Tóth et al., "Anomalous price impact and the critical nature of liquidity" (2011). Clark (1973); Ané, Geman (2000) — subordination. Bray, Majumdar, Schehr, "Persistence and first-passage properties in nonequilibrium systems," *Adv. Phys.* (2013); Redner, *A Guide to First-Passage Processes* (2001). Majumdar, Ziff, record statistics (2008); Godrèche, Majumdar, Schehr review (2017); Wergen, Krug (2011). Magdon-Ismail et al., "On the maximum drawdown of a Brownian motion" (2004); Chekhlov, Uryasev, Zabarankin, CDaR (2005); Goldberg, Mahmoud, CED (2016); Grossman, Zhou (1993). Johansen, Sornette, drawdown outliers (2001). Parkinson (1980); Garman–Klass (1980); Rogers–Satchell (1991); Yang–Zhang (2000). Rice, "Mathematical analysis of random noise" (1944–45). Simonsen, Jensen, Johansen, inverse statistics and gain–loss asymmetry (2002). Evans, Majumdar, "Diffusion with stochastic resetting," *PRL* (2011).




