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

**The efficiency paradox, stated up front.** Decompose each return into a sign and a magnitude, $r_t = \varepsilon_t\,|r_t|$. Empirically, in essentially every liquid market:

1. The signs of **order flow** (was the aggressor a buyer or a seller?) are enormously **persistent** — their autocorrelation decays as a slow power law over hours or days.
2. The signs of **price returns** are almost **unpredictable** — their autocorrelation is tiny beyond a few ticks. This is **(weak-form) market efficiency**: you cannot forecast the next tick's direction from past directions well enough to beat costs.

These two facts seem contradictory: if the flow that pushes prices is predictable, why isn't the price? The resolution — that the market's **response to flow** decays in a precisely tuned way that cancels the flow's persistence — is the intellectual spine of modern microstructure, and it is stated entirely in terms of the **path functionals** of this report. We will arrive at it properly in Part 5; everything before is the equipment needed to understand it.

**Notation used throughout.** $S_t$ is a price (mid-quote unless stated). Log-returns are $r_t=\ln S_t-\ln S_{t-1}$. The sign of the $t$-th event is $\varepsilon_t\in\{-1,+1\}$ — for returns, $\varepsilon_t=\operatorname{sgn}(r_t)$; for order flow, $\varepsilon_t=+1$ for a buyer-initiated trade, $-1$ for a seller-initiated one. Running extrema are $M_t=\max_{s\le t}S_s$ and $m_t=\min_{s\le t}S_s$. The drawdown is $D_t=M_t-S_t$ (or in log terms $\ln M_t - \ln S_t$). The number of crossings of level $u$ in $[0,T]$ is $N_T(u)$; the first-passage time to $u$ is $\tau_u=\inf\{t:S_t\ge u\}$. Expectations under a null model are written $\mathbb{E}_0[\cdot]$.

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
