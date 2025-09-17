---
tags:
---

# CSIRO Grassland Fire Spread

> [!abstract] What this page does
> - Summarises the CSIRO grassland rate-of-spread (ROS) model in plain language (Aus English).
> - Shows the full set of equations and how each piece fits together.
> - Provides a **placeholder** block where you’ll paste the interactive DataviewJS panel (sliders for wind, temperature, humidity, curing, pasture type, and optional fuel moisture override).
> - Offers guidance on interpreting results and known limits.

---

## Model overview

In the 1990s, ROS and the grassland fire danger rating were treated as separate problems. The CSIRO model predicts the **potential quasi-steady** rate of spread in grasslands as a function of pasture type, wind speed, dead-fuel moisture content, and degree of curing (how much dead grass is present).

$$R = f(i, U, m, C)$$

- $R$ — potential quasi-steady rate of spread (km/h)  
- $i$ — pasture type (cut/grazed vs natural)  
- $U$ — wind speed (km/h)  
- $m$ — moisture content of **dead** grass (%)  
- $C$ — degree of curing (% dead grass)

The model is constructed as multiplicative components:

![[Eq - CSIRO Grassland Model#^95b000]]


- $f(U)$ — wind response function  
- $\phi_m$ — fuel-moisture coefficient (depends on $m$ and $U$)  
- $\phi_C$ — curing coefficient

**Pasture type:** For **natural** pasture, the model applies a factor of $1.18$ relative to cut/grazed:

![[Eq - CSIRO Grassland Model#^820746]]


---

## 2) Equations

### 2.1 Wind response $f(U)$ (cut/grazed)

Critical behaviour is assumed at $U=5$ km/h: linear below and a power law above.

![[Eq - CSIRO Grassland Model#^c42420]]


For **natural** pasture: multiply final $R$ by $1.18$ (do **not** change $f(U)$ itself).

---

### 2.2 Fuel moisture coefficient $\phi_m(m,U)$

Below $12\%$ moisture, spread decreases exponentially with $m$; above $12\%$ it’s piecewise-linear down to an **extinction moisture** that depends on wind:

- If $U \le 10$ km/h, extinction at $20\%$  
- If $U > 10$ km/h, extinction at $24\%$

![[Eq - CSIRO Grassland Model#^ac834d]]


> [!tip] Estimating $m$ from weather  
> If you don’t have a direct moisture measurement, the model uses:  
> ![[Eq - CSIRO Grassland Model#^383dc0]]
> where $T$ is air temperature (°C) and $H$ is relative humidity (%).

---

### 2.3 Curing coefficient $\phi_C(C)$

Fires generally won’t spread for $C<20\%$. Above that, a sigmoidal response is assumed:

![[Eq - CSIRO Grassland Model#^d16bca]]


---

### Symbol Glossary
the following is a symbol glossary for this model:

![[Eq - CSIRO Grassland Model#^1b0f44]]
## Interactive panel
Test out the CSIRO fire spread model below.

![[CSIRO Grassland Slider]]

Try the presets for some different scenarios.

---

## How to read the outputs

* **$f(U)$**: The wind-only term for **cut/grazed** pasture before multiplying by $\phi_m$ and $\phi_C$.
* **$\phi_m$**: Shrinks with higher dead-fuel moisture; drops to \~0 near extinction moisture (20% for light winds, 24% for stronger winds).
* **$\phi_C$**: Near zero below $C=20\%$, increases sigmoidally as curing rises.
* **$R_{\text{cg}}$**: Final ROS for cut/grazed.
* **$R_{\text{n}} = 1.18,R_{\text{cg}}$**: Final ROS for natural pasture. 
 
> [!example] Quick scenarios to try
>
> * **Low curing:** $C=10%$ → $\phi_C \approx 0$ → little to no spread regardless of wind.
> * **Near moisture extinction:** Set $U=8$ km/h and $m=20%$ → $\phi_m \to 0$.
> * **Wind threshold:** Compare $U=5$ vs $U=6$ km/h to see the shift from linear to power-law.

---

## Worked example (step-by-step)

Suppose **cut/grazed** pasture with:
$U=12$ km/h, $C=60\%$, $T=30^\circ$C, $H=30\%$.

1. Estimate $m$:

   $$
   m = 9.58 - 0.205(30) + 0.138(30) \approx 7.57\%.
   $$

2. Compute $f(U)$ ($U>5$):

   $$
   f(12) = 1.1 + 0.715(12-5)^{0.844}.
   $$

3. Compute $\phi~m$ ($m \le 12$):

   $$
   \phi_m = \exp(-0.108 \times 7.57).
   $$

4. Compute $\phi~C$:

   $$
   \phi_C = \frac{1.036}{1 + 103.99 \exp\!\big(-0.0996\,(60 - 20)\big)}.
   $$

5. Final $R$ (cut/grazed):

   $$
   R_{\text{cg}} = f(12)\,\phi_m\,\phi_C.
   $$

6. Natural pasture:

   $$
   R_{\text{n}} = 1.18\,R_{\text{cg}}.
   $$

---

## Troubleshooting

* **ROS shows \~0**: Check if $C<20\%$ or $m$ is near extinction (20% for $U\le 10$, 24% for $U>10$).
* **Results jump around**: Ensure wind is in km/h and humidity/moisture are in %.
* **Counter-intuitive moisture**: Toggle the manual $m$ override to test sensitivity vs the $m(T,H)$ proxy.