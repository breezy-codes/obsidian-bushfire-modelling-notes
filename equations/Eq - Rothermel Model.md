---
tags:
  - mathematics/equations/mathematical-modelling
  - mathematics/equations/mathematical-modelling/bushfire-models
---

### Energy Balance
$$\Delta \cdot H = - \frac{d(\rho_{be}Q)}{dt}$$  
^d848a9

Energy conservation in a control volume just ahead of the flame front. 

---

### Heat Flux Vector
$$\mathbf{H} = H_x \hat{i} + H_y \hat{j} + H_z \hat{k}$$   ^326348

Heat flux components in the $x$, $y$, and $z$ directions.

---

### Boundary Conditions
At $x \to -\infty$:  

$$\mathbf{H} = 0, \quad Q = 0$$   ^438b51

At ignition surface $x=0$:  

$$\mathbf{H} = H_{ig}, \quad Q = Q_{ig}$$ ^7aba03

Initial and ignition states of the fuel. 

---

### Wide Fire Front Assumption
$$\frac{\partial H_y}{\partial y} = 0$$ ^29310b

Assumes lateral heat transfer is negligible. 

---

### Heat Absorption Rate
$$\frac{\partial Q}{\partial t} = -R \frac{\partial Q}{\partial x}$$   ^1cd6fc

Relates heat absorption to the fire’s rate of spread $R$.

---

### Conservation of Energy in $x$–$z$ Plane
$$-R \frac{\partial Q}{\partial x} = \frac{\partial H_x}{\partial x} + \frac{\partial H_z}{\partial z}$$   ^4cdffc

Energy input balances heat absorbed.

---

### Integrated Rate of Spread
$$R = \frac{1}{\rho_{be} Q_{ig}} \left\{ H_{ig} + \int_{-\infty}^0 \frac{\partial H_z}{\partial z} dx \right\}$$   ^31bad3

Spread rate expressed in terms of forward and vertical heat fluxes.

---

### Baseline Heat Flux
$$R = \frac{H_{p,0}}{\rho_{be} Q_{ig}}$$ ^266b67

Simplified form with no wind or slope.   

---

### Baseline Spread Rate
$$R_0 = \frac{H_{p,0}}{\rho_{be} Q_{ig}}$$ ^4cee53

Intrinsic spread rate depending only on fuel properties. 

---

### Wind Effect
$$\phi_w = C U^B$$   ^811a41

Empirical adjustment for wind speed $U$.

---

### Slope Effect
$$\phi_s = A \tan^2 \gamma_s$$   ^9cec8e

Empirical adjustment for slope angle $\gamma_s$.

---

### Final Rothermel Model
$$R = R_0 (1 + \phi_w + \phi_s)$$   ^48dc52

Overall fire spread rate including wind and slope effects.


# Symbol Glossary

> [!neutral]
>- $\mathbf{H}$ — Heat flux vector  
>- $H_x, H_y, H_z$ — Heat flux components in $x$, $y$, $z$ directions  
>- $H_{ig}$ — Forward heat flux at ignition  
>- $H_{p,0}$ — Baseline propagating heat flux (no wind/slope)  
>- $Q$ — Net heat absorbed per unit volume  
>- $Q_{ig}$ — Heat required for ignition per unit volume  
>- $\rho_{be}$ — Effective bulk density of fuel  
>- $R$ — Rate of spread (ROS) of the fire front  
>- $R_0$ — Baseline ROS without wind or slope  
>- $\phi_w$ — Wind coefficient  
>- $\phi_s$ — Slope coefficient  
>- $U$ — Mid-flame wind speed  
>- $\gamma_s$ — Slope angle  
>- $A, B, C$ — Empirical constants (fuel-dependent)

^787e1b

