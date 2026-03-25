---
doi: 10.yourblog20260325107
title: 'Global Oil and Gas Markets Logistics Dynamics and Geopolitical Shocks'
subtitle: ""
date: 2026-03-25T20:34:57+05:30
draft: true

# Primary categorization (1-3 max)
categories:
  - ""
  - ""
  - ""

# Specific tags (5-10, SEO + discoverability)
tags:
  - ""
  - ""
  - ""
  - ""
  - ""

# Book series (for multi-part articles)
series: [""]

# Rendering flags (your MathJax setup)
math: true
toc: true

# PaperMod feature image (from your generated cover)
cover:
  image: "" # e.g., /images/posts/your-image.png
  alt: ""
  caption: ""

# SEO / discoverability
description: ""
keywords: [""]

# Optional: social sharing
twitter: "quantatrisk"
github: "quantatrisk/quantatrisk.github.io"
---
doi: 10.yourblog20260325107

# Global Oil and Gas Markets: Logistics, Quantitative Dynamics, and Geopolitical Shocks


## 1\. Introduction: The Fragile Intersection of Molecules and Mathematics

The global oil and gas markets represent the most complex, heavily capitalized, and strategically vital commodity networks in the contemporary economic system. Over the past several decades, hydrocarbons have evolved from purely physical commodities traded at arm's length into highly sophisticated financial assets with trading horizons extending over a decade forward.

Operating at the intricate intersection of physical molecular flow and abstract financial derivatives, these markets dictate macroeconomic stability, shape geopolitical alignments, and drive continuous innovations within the field of quantitative finance. The inherent volatility of energy prices and the massive capital requirements of the supply chains have necessitated the creation of robust hedging mechanisms. Today, the nominal value of paper barrels traded on exchanges dwarfs the physical production of crude oil, attracting a diverse array of participants, from pension funds to macro hedge funds.

However, the financialization of energy cannot completely decouple the asset from its physical realities. Energy logistics rely on a fragile network of pipelines, maritime chokepoints, and highly specialized infrastructure. When geopolitical shocks occur, the resulting supply chain disruptions immediately transmit massive volatility spikes into the derivative markets. Understanding the modern energy landscape requires a multidisciplinary approach: analyzing the microstructure of crude pricing, modeling supply chain optimization through linear programming, and applying advanced mathematics to quantify the tail risks of geopolitical warfare.

## 2\. Market Microstructure and Supply Chain Logistics**

### 2.1\. Crude Oil Quality and Pricing Benchmarks

Crude oil is a highly heterogeneous, non-standard commodity. There are over 400 traded grades worldwide, each distinguished by its specific gravity (measured in degrees API) and sulfur content. Light, sweet crudes yield larger quantities of high-value refined products like motor gasoline and aviation fuel, whereas heavy, sour crudes require sophisticated, capital-intensive cracking units to remove sulfur dioxide and break long hydrocarbon chains into lighter fractions.

Because outright price volatility typically exceeds the variance in quality differentials, the global market relies on a narrow set of highly liquid "marker" grades or benchmarks.

| Benchmark Stream | Region of Origin | API Gravity | Sulfur Content (%) | Primary Logistics Base |
| :---- | :---- | :---- | :---- | :---- |
| West Texas Intermediate (WTI) | United States | 38.0 \- 40.0 | 0.30 | Pipeline delivery at Cushing, Oklahoma |
| Brent BFO (Brent, Forties, Oseberg) | North Sea (UK/Norway) | 38.0 | 0.30 | Seaborne cargoes (FOB Sullom Voe/North Sea) |
| Dubai / Oman | Middle East | 32.5 | 1.68 \- 2.00 | Seaborne cargoes (FOB Fateh/Kerteh) |
| Bonny Light | Nigeria | 37.6 | 0.13 | Seaborne cargoes |
| Urals | Russia | 31.0 \- 32.0 | 1.20 \- 1.40 | Pipeline and Seaborne (Novorossiysk) |

Physical cargoes of various global grades are routinely priced at a floating differential to these indices. For instance, a cargo of Russian Urals crude might be sold at a formula of "Dated Brent minus $1.50 per barrel". The buyer simultaneously hedges the outright price risk using ICE Brent Futures or OTC swaps, allowing the market to cleanly separate the systemic risk of the global oil price from the idiosyncratic quality and location differentials.

### 2.2\. Operational Logistics and the Hierarchy of Decisions

Logistics planning within a consortium of oil companies involves coordinating supply, transformation, storage, and transportation across a highly complex network. This physical network links continuous-flow pipelines with discrete transport modalities like ships and trucks, operating across discrete time scales.36 To manage these diverse, interconnected activities, firms rely on a distinct hierarchy of decision-making: long-term strategic planning, medium-term tactical planning, and day-to-day operational logistics.36

During any given time period, the primary activities that must be executed include:

* **Supply:** Operators are responsible for delivering predetermined volumes of crude oil to specified depots, governed by strict contractual limits decided by the consortium to ensure system compliance.36  
* **Transformation (Refining):** The conversion of raw crude into usable fuel products occurs strictly at designated refinery nodes within the network. The feedstocks for this transformation include newly supplied volumes as well as existing crude inventories carried over from prior periods.36  
* **Transportation:** Crude volumes are routed directly to refinery facilities for processing or moved to alternative depots. Subsequently, the refined end-products are distributed from refineries to various storage depots across the system.36 Mathematically, this physical infrastructure is modeled as a directed graph, where depots and refineries represent "nodes," and the transport mechanisms connecting them function as "arcs" or links.36  
* **Stocking (Storage):** Both the incoming crude oil and the refined output products are held in temporary storage at various network depots until they are required for transformation or final market distribution.36

Individual firms within a consortium pool their raw materials, refining assets, and transportation networks to satisfy aggregate demand.36 However, structural constraints—such as limits on pipeline throughput or refinery capacity—can lead to logistical imbalances and mathematical infeasibilities.36 When internal infrastructure cannot adequately balance supply with consumer demand, the consortium must turn to external commodity markets:

* **Spot Market Purchasing:** If any node experiences a deficit in crude supply or storage during a specific period, the consortium intervenes by acquiring additional crude at prevailing spot market rates.36  
* **Spot Market Selling:** Conversely, if the consortium holds excess refined product inventory at any node, it can liquidate these unneeded volumes on the spot market.36

Because these operations encompass a diverse array of products (a multicommodity flow problem) and require integer-based solutions to account for discrete transport modalities like trucks, the computational complexity of the associated mathematical models is massive.36 To resolve this, analysts split the problem into a two-level hierarchical framework:36

* **Strategic/Tactical Level:** Optimization algorithms determine the macro-level petroleum activities required to minimize the consortium's total operational costs.36  
* **Operational Level:** Utilizing the targets set in the strategic phase, detailed, day-to-day schedules for each transport modality are crafted over specific, implementable periods, typically assuming deterministic demand and pricing parameters.36

### 2.3\. Quantitative Modeling of Petroleum Logistics

The sheer complexity of the aforementioned logistics—balancing continuous pipeline flows, discrete maritime cargoes, and seasonal demand fluctuations—requires rigorous mathematical modeling. The industry standard relies on Mixed-Integer Linear Programming (MILP) and stochastic optimization algorithms to maximize net present value, minimize transportation costs, and ensure supply chain resilience.

The Depot and Refinery Optimization Problem (DROP) serves as a foundational mathematical framework for the strategic planning of downstream oil supply chains.1 The objective function of the DROP model seeks to minimize the total operational cost ![][image1], over a specified discrete time horizon ![][image2], while meeting exogenous product demands.1 The cost function aggregates raw material supply, industrial transformation, transportation logistics, storage, and spot market interventions 1:

\begin{align}  \tag{2.1}
g(X, Z, S, X^{spot}, Y^{spot}) = \sum_{n \in N} \sum_{\omega \in \Omega} \sum_{p \in P} \sum_{t \in T} C_{n,p,t} X_{n,\omega,p,t} \qquad \text{SUPPLY} \\
 +\sum_{r \in R} \sum_{\omega \in \Omega} \sum_{f \in F} \sum_{s \in S} C_{r,f,s,\omega} Z_{r,f,s,\omega} \quad \text{REFINING} \\
+\sum_{p \in P} \sum_{m \in M} \sum_{p' \in P} \sum_{(i,j) \in A} \sum_{\omega \in \Omega} \sum_{t \in T} C_{i,j,p,m,p',\omega,t} E_{i,j,p,m,p',\omega,t} \qquad \text{TRANSPORTATION} \\
+\sum_{n \in N} \sum_{p \in P} \sum_{t \in T} C_{n,p,t}^{S} S_{n,p,t} \qquad \text{STOCK} \\
+\sum_{n \in N} \sum_{p \in P} \sum_{t \in T} C_{n,p,t}^{spot} X_{n,p,t}^{spot} \qquad \text{SPOT PURCHASE} \\
-\sum_{n \in N} \sum_{p \in P} \sum_{t \in T} P_{n,p,t} Y_{n,p,t}^{spot} \quad \text{SPOT SALE}
\end{align}

To account for market uncertainty, quantitative analysts utilize stochastic extensions (like DROPS). This approach employs a general-purpose stochastic problem generator, such as STOCHGEN, to create an expansive scenario tree representing the unfolding future.2 Random fluctuations in market demands $D$, spot costs for crude $C$, and refined product prices $P$ are simulated using Brownian motion processes, where the state variable transitions are modeled as $V(D, C, P) = \mu(D, C, P) + \sigma(D, C, P)dZ$. 1 This yields first-stage decisions that are optimally hedged against a wide probability distribution of future scenarios.1

## **3\. Chokepoints and Trade Routes: The Geography of Risk**

The mathematical models governing energy logistics are entirely constrained by the physical geography of global trade routes. The international seaborne trade of oil and gas is heavily dependent on a few critical maritime chokepoints.

The Strait of Hormuz is the world's most critical energy artery. At its narrowest, it consists of 2-mile-wide navigable channels.3 Under normal conditions, approximately 20 million barrels of petroleum liquids per day—roughly 20% to 25% of global seaborne oil trade—transit this corridor.3 Furthermore, it handles nearly 20% of the world's LNG supply, predominantly originating from Qatar and the UAE.3 Alternative logistical routes are severely limited; pipeline bypass capacities, such as Saudi Arabia's East-West pipeline, offer only 3.5 to 5.5 million bpd, leaving a massive volumetric deficit that cannot be cleared if the strait is compromised.5

As traditional chokepoints become increasingly congested and politically fraught, logistics optimization models are beginning to weight alternative corridors, notably the Northern Sea Route (NSR). Enabled by declining Arctic sea ice, the NSR allows vessels to transit from Europe to Asia along Russia's northern coast.6 While this route cuts voyage times significantly, the NSR remains a highly specialized, seasonal niche route limited by the absolute necessity of icebreaker escorts and extreme weather risks, making it currently unable to fully offset the volume risks concentrated in the Middle East.7

## **4\. Trade Policies and Shifting Geoeconomics**

Logistics are not solely disrupted by physical bottlenecks; trade policies and economic sanctions play a massive role in rewiring global supply chains.

The escalating trade war between the United States and China fundamentally altered LNG logistics. The implementation of reciprocal tariffs, including steep duties on Chinese imports, forced Chinese buyers to actively avoid US-sourced LNG.8 Chinese state-owned enterprises utilized the destination flexibility inherent in US contracts to reroute cargoes to European markets.8 They simultaneously backfilled domestic demand with heavily discounted, sanctioned Iranian oil; by the end of 2025, China was importing roughly 1.4 million barrels per day from Iran, processed primarily by small, private "teapot" refineries and paid for in renminbi via the Cross-border Interbank Payment System (CIPS) to circumvent Western financial networks.9

Concurrently, the global natural gas supply chain faces a novel and highly inelastic demand vector: the proliferation of Artificial Intelligence (AI) and the massive build-out of hyperscale data centers. Meeting the power demand from AI will likely require the United States to increase natural gas production by 10% to 15% by the early 2030s.10 Because renewable energy generation struggles with inherent intermittency, natural gas has become the balancing fuel of choice to guarantee continuous uptime for AI clusters.11 Quantitative projections indicate this domestic demand surge directly competes with expanding US LNG export terminals, pushing the upper bounds of the EIA's confidence interval for natural gas prices to $10 per MMBTU by 2027, baking a persistent volatility premium into long-dated derivatives.11

## **5\. Historical Case Studies in Geopolitical Oil Shocks**

Analyzing historical supply shocks provides critical empirical data for calibrating tail-risk probabilities in modern models.

* **The 1956 Suez Crisis and the 1973 OPEC Embargo:** The nationalization of the Suez Canal Company by Egypt's Gamal Abdel Nasser and subsequent military interventions forced vessels to reroute around the Cape of Good Hope, lengthening supply chains and sparking the IMF's first major episodic international lending intervention.12 Later, the 1973 OPEC embargo removed 4.4 mb/d (roughly 7.5% of global output) from the market.15 Prices doubled almost instantaneously, highlighting the extreme inelasticity of short-term oil demand and triggering widespread stagflation.15  
* **The Tanker War (1980-1988):** During the Iran-Iraq War, both nations deliberately targeted commercial shipping in the Persian Gulf.16 Surprisingly, the macroeconomic impact on global oil prices was relatively muted. Retrospective analysis resolves this paradox by noting that only 1% to 2% of total shipping passing through the Strait was actually damaged, and major supply lines remained largely operational.18  
* **The 2022 Russia-Ukraine Shock:** The invasion of Ukraine forced a massive rewiring of global logistics, serving as a template for modern macro-contagion where damage rapidly spread across asset classes.19 Russian crude was diverted on long-haul maritime routes to Asia. Simultaneously, the curtailment of Russian pipeline gas to Europe sent TTF natural gas prices to historic highs, proving that regional gas markets were now inextricably linked by global seaborne trade.

Macroeconomists modeling these events emphasize the dampening effect of storage. In Dynamic Stochastic General Equilibrium (DSGE) models, the presence of oil storage and precautionary inventories acts as a massive mathematical dampener against time-varying geopolitical risk.20 When a supply shock occurs, rational economic agents draw down inventories to meet immediate demand, smoothing the macroeconomic blow.20

## **6\. The 2026 US-Iran-Israel Conflict: A Structural Shock**

All existing models of supply chain resilience were severely stress-tested on February 28, 2026, when the United States and Israel launched a massive military campaign against Iranian targets, resulting in the death of Iran's Supreme Leader.21 The geopolitical explosion rapidly cascaded into what energy watchdogs characterized as the largest single supply disruption in the history of oil markets.23

### 6.1\. The Strait of Hormuz Blockade**

In retaliation, Iran launched missile and drone barrages and issued warnings prohibiting vessel passage through the Strait of Hormuz.22 The withdrawal of maritime war-risk insurance coverage, coupled with the physical threat, brought commercial transit to a standstill, leaving approximately 150 tankers anchored outside the Strait, unable to load or deliver cargo.5

| Commodity Type | Global Seaborne Trade % via Hormuz | Most Vulnerable Regions/Markets |
| :---- | :---- | :---- |
| Crude Oil & Condensate | 30.7% (13.37 mb/d) | Asia (China, India, Japan) |
| Liquefied Natural Gas (LNG) | 19.0% \- 20.0% | Asia, European Grid |
| Jet Fuel / Kerosene | 19.4% (378 kb/d) | Europe (38.9% dependency) |
| Liquefied Petroleum Gas (LPG) | 29.0% | India (85.0% dependency) |
| Gasoil / Diesel | 10.3% (716 kb/d) | Africa, Europe |

*Data reflecting market dependencies and at-risk volumes during the March 2026 crisis.* 5

<div style="display: flex; gap: 2rem; justify-content: center; flex-wrap: wrap;">
  {{< figure src="/images/strait-of-hormuz-feb27-2026.webp" width="500" height="300" caption="Caption for Image 1" >}}
  {{< figure src="/images/strait-of-hormuz-march2-2026.webp" width="500" height="300" caption="Caption for Image 2" >}}
</div>
    
{{< video src="/videos/satellite-tracking-data-hormuz.mp4" width="800px" height="450px" caption="Default 16:9" >}}

This blockade effectively severed 20 million barrels of petroleum liquids per day and 20% of the world's LNG supply from the market.22 As regional storage tanks quickly reached maximum capacity, producers were forced into emergency production shut-ins of nearly 7 million barrels per day (mbd), transitioning the crisis from a logistical delay to a permanent loss of physical molecules.26

### 6.2\. The Derivative Market Reaction and Policy Response**

Brent crude futures violently breached the $100/bbl threshold on March 8, spiking to an intraday peak near $126/bbl.23 The term structure of the futures curve snapped into a state of extreme backwardation as the convenience yield for immediate physical possession far exceeded storage costs.28 The WTI-Brent spread blew out as global refiners aggressively bid up Brent to hedge supply exposure.5

The extreme volatility triggered severe liquidity issues within derivative markets. Market participants whose trades fell out of the money faced massive margin calls, forcing institutional investors into indiscriminate liquidation across unrelated asset classes to raise cash, bridging the gap between a commodity shock and a broader financial crisis.30

To mitigate the catastrophic economic fallout, the IEA ordered the largest release of government reserves in its history on March 11, unleashing 400 million barrels of crude, including 172 million barrels from the US Strategic Petroleum Reserve (SPR).23 Furthermore, to stabilize global supply, the US Treasury temporarily eased energy sanctions, allowing stranded Russian oil to flow into the market until April 11, 2026\.32 Meanwhile, the OPEC+ coalition faced a logistical paradox: while they held an emergency meeting on March 1 and agreed to a modest output hike of 206,000 barrels per day starting in April, the vast majority of their 3.5 million bpd in spare capacity was geographically trapped behind the blockaded Strait of Hormuz, rendering the paper quota increase largely ineffective.5

### 6.3\. Knightian Uncertainty and the Limits of Quantitative Forecasting**

The 2026 conflict and the subsequent blockade plunge global energy markets into a state of "Knightian Uncertainty." First formalized by economist Frank Knight in 1921, this concept fundamentally distinguishes between quantifiable "risk"—situations where the range of potential outcomes and their underlying probability distributions are mathematically known—and true "uncertainty," where the probabilities of specific future events cannot be scientifically calculated or measured.

While standard risk assessment tools and derivative pricing models rely heavily on historical data and diffusion processes to predict market fluctuations, the unprecedented magnitude of a complete Hormuz blockade means that traditional models face severe operational limitations. Because the geopolitical sphere is radically unpredictable, the cascading macroeconomic effects of a 20 million barrel per day supply deficit create systemic outcomes that are exceptionally difficult to quantify. Consequently, market participants and central banks must acknowledge that they are operating in an environment of radical ambiguity. This paradigm shift renders standard continuous volatility models inadequate, elevating the importance of qualitative scenario analysis and robust contingency frameworks to manage extreme, unquantifiable systemic risk.

### 6.4\. Quantitative Models for Tail Risk

To accurately manage the measurable risk of these sudden discontinuous price spikes, quantitative risk managers rely on Jump-Diffusion models.34 These models combine standard continuous Brownian motion with a discrete, Poisson-driven jump process to capture sudden events, heavily utilized for pricing deep out-of-the-money options during crises.34 Additionally, researchers employ Conditional Autoregressive Value at Risk (CAViaR) frameworks to model the extreme tail of the return distribution.35 Empirical testing proves that negative geopolitical news generates a statistically stronger increase in oil tail risk compared to stabilizing developments, highlighting the leverage effect and loss aversion characteristic of extreme systemic stress.35

## 7\. Conclusion: Key Takeaways

* **Financial vs. Physical Divergence:** The highly sophisticated financialization of energy markets rests on a rigid, highly constrained physical logistics network. While derivatives can hedge price risk, they cannot hedge the physical loss of molecules during a chokepoint blockade.  
* **The Mathematical Imperative of Storage:** Quantitative DSGE models and real-world historical case studies consistently prove that strategic petroleum reserves (SPRs) act as the primary macroeconomic dampener against sudden, time-varying supply shocks.  
* **The Hormuz Bottleneck is Unsolvable in the Short Term:** The 2026 conflict proved that alternative pipelines cannot offset the 20 million bpd deficit caused by a Strait of Hormuz closure, rendering even OPEC+ spare capacity useless if ships cannot sail.  
* **Geopolitics Drives Contagion:** Sudden energy price jumps trigger massive margin calls in derivative clearinghouses. This forces cross-asset liquidation, meaning an energy logistics failure rapidly evolves into a systemic financial liquidity crisis.  
* **New Demand Vectors Add to Volatility:** The structural shift driven by AI data centers requiring baseload natural gas power guarantees that future supply shocks will occur in a market that already has an elevated, persistent volatility premium.

#### **Works cited**

1. (PDF) Planning Logistics Operations in the Oil Industry, accessed March 17, 2026, [https://www.researchgate.net/publication/245279096\_Planning\_Logistics\_Operations\_in\_the\_Oil\_Industry](https://www.researchgate.net/publication/245279096_Planning_Logistics_Operations_in_the_Oil_Industry)  
2. Supply chain optimization of petroleum organization under uncertainty in market demands and prices | Request PDF \- ResearchGate, accessed March 17, 2026, [https://www.researchgate.net/publication/4940167\_Supply\_chain\_optimization\_of\_petroleum\_organization\_under\_uncertainty\_in\_market\_demands\_and\_prices](https://www.researchgate.net/publication/4940167_Supply_chain_optimization_of_petroleum_organization_under_uncertainty_in_market_demands_and_prices)  
3. Strait of Hormuz \- About \- IEA, accessed March 17, 2026, [https://www.iea.org/about/oil-security-and-emergency-response/strait-of-hormuz](https://www.iea.org/about/oil-security-and-emergency-response/strait-of-hormuz)  
4. Global Oil Market Implications of U.S.-Israel Attack on Iran \- AAF, accessed March 17, 2026, [https://www.americanactionforum.org/insight/global-oil-market-implications-of-u-s-israel-attack-on-iran/](https://www.americanactionforum.org/insight/global-oil-market-implications-of-u-s-israel-attack-on-iran/)  
5. US-Iran conflict: Strait of Hormuz crisis reshapes global oil markets \- Kpler, accessed March 17, 2026, [https://www.kpler.com/blog/us-iran-conflict-strait-of-hormuz-crisis-reshapes-global-oil-markets](https://www.kpler.com/blog/us-iran-conflict-strait-of-hormuz-crisis-reshapes-global-oil-markets)  
6. Arctic: The Next Global Power Battleground? \- The Diplomatic Insight, accessed March 17, 2026, [https://thediplomaticinsight.com/arctic-next-global-power-battleground/](https://thediplomaticinsight.com/arctic-next-global-power-battleground/)  
7. Arctic Shipping Potential Along the Northern Sea Route, accessed March 17, 2026, [https://www.thearcticinstitute.org/arctic-shipping-potential/](https://www.thearcticinstitute.org/arctic-shipping-potential/)  
8. U.S.-China Trade War and the Future of U.S. LNG \- CSIS, accessed March 17, 2026, [https://www.csis.org/analysis/us-china-trade-war-and-future-us-lng](https://www.csis.org/analysis/us-china-trade-war-and-future-us-lng)  
9. What the war in Iran means for China \- Bruegel.org, accessed March 17, 2026, [https://www.bruegel.org/analysis/what-war-iran-means-china](https://www.bruegel.org/analysis/what-war-iran-means-china)  
10. Powering Data Centers Will Require Rapid Increases In Natural Gas Production, accessed March 17, 2026, [https://www.aogr.com/web-exclusives/exclusive-story/powering-data-centers-will-require-rapid-increases-in-natural-gas-production](https://www.aogr.com/web-exclusives/exclusive-story/powering-data-centers-will-require-rapid-increases-in-natural-gas-production)  
11. Projected Impact of AI & Crypto-Driven Demand on Natural Gas Prices | Stout, accessed March 17, 2026, [https://www.stout.com/en/insights/commentary/projected-impact-ai-crypto-driven-demand-natural-gas-prices](https://www.stout.com/en/insights/commentary/projected-impact-ai-crypto-driven-demand-natural-gas-prices)  
12. Finance & Development, September 2001 \- Was Suez in 1956 the First Financial Crisis of the Twenty-First Century? \- IMF, accessed March 17, 2026, [https://www.imf.org/external/pubs/ft/fandd/2001/09/boughton.htm](https://www.imf.org/external/pubs/ft/fandd/2001/09/boughton.htm)  
13. Suez Crisis \- Wikipedia, accessed March 17, 2026, [https://en.wikipedia.org/wiki/Suez\_Crisis](https://en.wikipedia.org/wiki/Suez_Crisis)  
14. The Suez Crisis, 1956 \- Milestones in the History of U.S. Foreign Relations \- Office of the Historian, accessed March 17, 2026, [https://history.state.gov/milestones/1953-1960/suez](https://history.state.gov/milestones/1953-1960/suez)  
15. Historical Oil Shocks\* \- UC San Diego Department of Economics, accessed March 17, 2026, [https://econweb.ucsd.edu/\~jhamilton/oil\_history.pdf](https://econweb.ucsd.edu/~jhamilton/oil_history.pdf)  
16. Strait of Hormuz \- Tanker War \- The Strauss Center, accessed March 17, 2026, [https://www.strausscenter.org/strait-of-hormuz-tanker-war/](https://www.strausscenter.org/strait-of-hormuz-tanker-war/)  
17. The First Tanker War with Iran | History Today, accessed March 17, 2026, [https://www.historytoday.com/history-matters/first-tanker-war](https://www.historytoday.com/history-matters/first-tanker-war)  
18. An easy target? Types of attack on oil tankers by state actors, accessed March 17, 2026, [https://securityanddefence.pl/An-easy-target-nTypes-of-attack-on-oil-tankers-by-state-actors,118147,0,2.html](https://securityanddefence.pl/An-easy-target-nTypes-of-attack-on-oil-tankers-by-state-actors,118147,0,2.html)  
19. A Multi-Asset Playbook for Geopolitical Shocks and Oil Supply Disruption | MSCI, accessed March 17, 2026, [https://www.msci.com/research-and-insights/blog-post/a-multi-asset-playbook-for-geopolitical-shocks-and-oil-supply-disruption](https://www.msci.com/research-and-insights/blog-post/a-multi-asset-playbook-for-geopolitical-shocks-and-oil-supply-disruption)  
20. Geopolitical Oil Price Risk and Economic Fluctuations – Research ..., accessed March 17, 2026, [https://www.dallasfed.org/\~/media/documents/research/papers/2024/wp2403.pdf](https://www.dallasfed.org/~/media/documents/research/papers/2024/wp2403.pdf)  
21. Middle East conflict set to drive oil and LNG prices significantly higher | Wood Mackenzie, accessed March 17, 2026, [https://www.woodmac.com/news/opinion/middle-east-conflict-set-to-drive-oil-and-LNG-prices-significantly-higher/](https://www.woodmac.com/news/opinion/middle-east-conflict-set-to-drive-oil-and-LNG-prices-significantly-higher/)  
22. 2026 Strait of Hormuz crisis \- Wikipedia, accessed March 17, 2026, [https://en.wikipedia.org/wiki/2026\_Strait\_of\_Hormuz\_crisis](https://en.wikipedia.org/wiki/2026_Strait_of_Hormuz_crisis)  
23. Middle East war creating 'largest supply disruption in the history of oil markets', accessed March 17, 2026, [https://www.theguardian.com/business/2026/mar/12/middle-east-war-creating-largest-supply-disruption-in-the-history-of-oil-markets](https://www.theguardian.com/business/2026/mar/12/middle-east-war-creating-largest-supply-disruption-in-the-history-of-oil-markets)  
24. Strait of Hormuz Closure 2026: What It Means for Asian Oil Markets \- Statt, accessed March 17, 2026, [https://statt.com/blog/strait-of-hormuz-closure/](https://statt.com/blog/strait-of-hormuz-closure/)  
25. Geoeconomics Impact of the US-Israel War Against Iran \- SpecialEurasia, accessed March 17, 2026, [https://www.specialeurasia.com/2026/03/05/geoeconomics-us-israel-iran-war/](https://www.specialeurasia.com/2026/03/05/geoeconomics-us-israel-iran-war/)  
26. US–Israel Military Operation Against Iran: Are Markets on Edge?, accessed March 17, 2026, [https://www.jpmorgan.com/insights/global-research/commodities/iran-us-tensions-market-effect](https://www.jpmorgan.com/insights/global-research/commodities/iran-us-tensions-market-effect)  
27. Markets Confront the Economic Impact of the Iran Conflict, accessed March 17, 2026, [https://home.cib.natixis.com/articles/markets-confront-the-economic-impact-of-the-iran-conflict](https://home.cib.natixis.com/articles/markets-confront-the-economic-impact-of-the-iran-conflict)  
28. Backwardation in Comodity Market: Causes and Trading Strategies \- Gotrade, accessed March 17, 2026, [https://www.heygotrade.com/en/blog/backwardation-in-comodity-market](https://www.heygotrade.com/en/blog/backwardation-in-comodity-market)  
29. What is Backwardation and how does it impact the stock market? \- StoneX, accessed March 17, 2026, [https://www.stonex.com/en/financial-glossary/backwardation/](https://www.stonex.com/en/financial-glossary/backwardation/)  
30. Gulf conflict: Challenges affecting businesses in the energy sector \- Reed Smith LLP, accessed March 17, 2026, [https://www.reedsmith.com/media/elkj1wbz/gulf\_conflict\_-\_challenges\_affecting\_businesses\_in\_the\_energy\_sector\_-\_12\_march\_2026.pdf](https://www.reedsmith.com/media/elkj1wbz/gulf_conflict_-_challenges_affecting_businesses_in_the_energy_sector_-_12_march_2026.pdf)  
31. From fertilizer to soybeans: The fallout of war \- Farm Progress, accessed March 17, 2026, [https://www.farmprogress.com/commentary/from-fertilizer-to-soybeans-the-fallout-of-war](https://www.farmprogress.com/commentary/from-fertilizer-to-soybeans-the-fallout-of-war)  
32. Russian Offensive Campaign Assessment, March 13, 2026, accessed March 17, 2026, [https://understandingwar.org/research/russia-ukraine/russian-offensive-campaign-assessment-march-13-2026/](https://understandingwar.org/research/russia-ukraine/russian-offensive-campaign-assessment-march-13-2026/)  
33. OPEC+ hikes oil production by more than expected following outbreak of Iran war \- Euractiv, accessed March 17, 2026, [https://www.euractiv.com/news/opec-hikes-oil-production-by-more-than-expected-following-outbreak-of-iran-war/](https://www.euractiv.com/news/opec-hikes-oil-production-by-more-than-expected-following-outbreak-of-iran-war/)  
34. What Is a Jump Diffusion Model? \- CQF, accessed March 17, 2026, [https://www.cqf.com/blog/quant-finance-101/what-is-a-jump-diffusion-model](https://www.cqf.com/blog/quant-finance-101/what-is-a-jump-diffusion-model)  
35. Geopolitical Shocks and Crude Oil Market Tail Risk: Evidence from ..., accessed March 17, 2026, [https://www.mdpi.com/2227-7099/14/3/92](https://www.mdpi.com/2227-7099/14/3/92)  
36. Oil Logistics.pdf

