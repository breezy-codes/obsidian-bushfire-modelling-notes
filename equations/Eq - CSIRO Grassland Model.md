---
tags:
  - mathematics/equations/mathematical-modelling
  - mathematics/equations/mathematical-modelling/bushfire-models
---

### Rate of Spread
$$R = f(U)\,\phi_m(m,U)\,\phi_C(C)$$  ^95b000

ROS as a product of wind, moisture, and curing terms. For natural pasture, multiply by $1.18$.

---

### Wind Response $f(U)$
Cut/grazed pasture:  

$$
f(U) =
\begin{cases}
0.054 + 0.209U, & U \le 5 \\\\
1.1 + 0.715\,(U - 5)^{0.844}, & U > 5
\end{cases}
$$

^c42420


Natural pasture:  

$$R_{\text{natural}} = 1.18\,R_{\text{cut/grazed}}.$$ ^820746


---

### Fuel Moisture Coefficient $\phi_m(m,U)$
Piecewise form with extinction moisture depending on wind speed:


$$
\phi_m =
\begin{cases}
\exp(-0.108\,m), & m \le 12 \\\\
0.684 - 0.0342\,m, & m > 12,\; U \le 10 \\\\
0.547 - 0.0228\,m, & m > 12,\; U > 10
\end{cases}
$$

^ac834d


Moisture proxy from temperature $T$ and relative humidity $H$:  

$$m = 9.58 - 0.205T + 0.138H$$ ^383dc0


---

### Curing Coefficient $\phi_C(C)$

$$
\phi_C = \frac{1.036}{1 + 103.99 \exp\!\big(-0.0996\,(C - 20)\big)}
$$

^d16bca


---

> [!neutral] **Symbol Glossary**  
> - $R$ — Rate of spread (km/h)  
> - $U$ — Wind speed (km/h)  
> - $m$ — Dead-fuel moisture content (%)  
> - $C$ — Degree of curing (% dead grass)  
> - $T$ — Air temperature (°C)  
> - $H$ — Relative humidity (%)  
> - $f(U)$ — Wind response function  
> - $\phi_m$ — Fuel-moisture coefficient  
> - $\phi_C$ — Curing coefficient  
> - $R_{\text{natural}}$ — ROS for natural pasture  
> - $R_{\text{cut/grazed}}$ — ROS for cut/grazed pasture

^1b0f44

