---
aliases:
tags:
---

![[Def - The Rothermel model]]

# The Rothermel Model of Fire Spread

The **Rothermel model** is one of the most widely adopted mathematical models for predicting the **rate of spread (ROS)** of wildland fires. Developed in the early 1970s by Richard C. Rothermel of the USDA Forest Service, it represents a **semi-physical, semi-empirical approach** that balances physical principles (such as conservation of energy) with empirical adjustments to account for the complexities of real-world fire behaviour.

---

## Historical Context

- **Early work**: Before Rothermel, Fransden and others attempted to describe fire spread using the **conservation of energy principle**, but these approaches were limited in scope and accuracy.
- **Rothermel’s contribution**: He formulated a model that simplified the fuel bed into a homogeneous layer and then used a **control volume** (a differential element $\Delta V$) to track how heat transfer drives fire spread.
- **Impact**: The model became the backbone of operational fire spread simulators (e.g. **BEHAVE** in the US) and is still used internationally today.

---

## Conceptual Framework

Rothermel’s model views the advancing fire front as the result of **heat transfer into unburned fuel ahead of the flame**. For a control volume $\Delta V$ located just in front of the combustion zone:

![[Eq - Rothermel Model#^d848a9]]

where:
![[Eq - Rothermel Model#^326348]]
is the **heat flux vector**,
- $\rho_{be}$ is the **effective bulk density**, representing the mass of combustible material per unit volume that absorbs heat,
- $Q$ is the **net heat absorbed per unit volume** as $\Delta V$ approaches ignition.

### Boundary Conditions

- At $x \to -\infty$: the fuel is cold and unignited:  
![[Eq - Rothermel Model#^438b51]]
- At the ignition surface ($x=0$):  
![[Eq - Rothermel Model#^7aba03]]

Rothermel assumed the fire front is **sufficiently wide** that lateral heat flow is negligible:

![[Eq - Rothermel Model#^29310b]]

---

## Derivation of the Rate of Spread

Heat absorbed per unit volume changes with time as the control volume moves towards the fire front:

![[Eq - Rothermel Model#^1cd6fc]]

Here, $R$ is the **rate of spread** (ROS).

Conservation of energy gives:

![[Eq - Rothermel Model#^4cdffc]]

Integrating across the unburned region ($-\infty \to 0$):

![[Eq - Rothermel Model#^31bad3]]

The expression captures two sources of heat transfer:
1. **Forward heat flux ($H_{ig}$)** directly into the unburned fuel,
2. **Vertical heat flux ($H_z$)** integrated along the approach to the flame front.

---

## Baseline Model without Wind and Slope

Defining the **propagating heat flux** in the absence of external influences as $H_{p,0}$, Rothermel reduced the expression to:

![[Eq - Rothermel Model#^266b67]]

This baseline rate of spread is denoted:

![[Eq - Rothermel Model#^4cee53]]

It represents the **intrinsic spread rate** of a fire, determined solely by fuel properties (density, moisture, arrangement) and heat transfer.

---

## Influence of Wind and Slope

In reality, fire behaviour is strongly affected by **wind** and **terrain slope**:

- **Wind ($\phi_w$):** tilts flames forward, increasing heat transfer to unburned fuel.
![[Eq - Rothermel Model#^811a41]]
- **Slope ($\phi_s$):** similarly tilts flames upslope, concentrating radiant and convective heating.
![[Eq - Rothermel Model#^9cec8e]]

Rothermel introduced **dimensionless empirical coefficients** to account for these effects where:

- $U$ = mid-flame wind speed,
- $\gamma_s$ = slope angle,
- $A$, $B$, $C$ = empirically determined constants based on fuel type and conditions.

---

## Final Form of the Rothermel Model

Combining all effects, the final model is expressed as:

![[Eq - Rothermel Model#^48dc52]]

This simple structure highlights the **multiplicative impact** of environmental factors on the intrinsic spread rate.

---

## Symbol Glossary
To make it easier, here is the symbol glossary -
![[Eq - Rothermel Model#^787e1b]]

---

## Interactive panel
Test out the Rothermel model below.

![[Rothermel Model Slider]]

## Strengths and Limitations

**Strengths:**
- Physically grounded in **energy conservation**.
- Widely validated and implemented in fire spread simulators.
- Provides a flexible framework for incorporating **fuel, wind, and slope effects**.

**Limitations:**
- Assumes a **homogeneous fuel bed**, which is rarely true in complex landscapes.
- Ignores **feedbacks with the atmosphere** (e.g. fire-induced winds, plume dynamics).
- Does not account for **spotting, crown fires, or long-range ember transport**.
- Wind and slope factors are purely empirical, requiring calibration for each fuel type.

---

## Summary

The **Rothermel model** remains the cornerstone of operational fire behaviour prediction. Its **semi-empirical foundation** makes it tractable and adaptable, though at the cost of simplification. Modern models (such as coupled fire–atmosphere models) extend beyond Rothermel’s assumptions, but the **core energy-balance principle** he introduced continues to underpin wildfire science and management.

![[Eq - Rothermel Model#^48dc52]]

