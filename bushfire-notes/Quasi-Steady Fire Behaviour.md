---
aliases:
tags:
---

The concept of **quasi-steady fire behaviour** plays an important role in wildfire science and modelling. Fires are inherently dynamic and subject to constant change in wind, fuel, and topography. However, to make progress in theory and prediction, researchers often assume that the fire front achieves a **quasi-steady state**, where its behaviour can be treated as *approximately steady* over short timescales. This simplifying assumption is known as the **quasi-steady ansatz**.

---

## Fire Behaviour: Transient vs Steady

### Transient Fire Behaviour
- Fires are naturally **unsteady systems**.  
- Wind gusts, turbulence, varying fuel loads, moisture patches, and slope changes continually disturb the flame front.  
- In this state, the **rate of spread (ROS)**, flame geometry, and intensity fluctuate significantly.  

### Steady Fire Behaviour
- A **theoretical steady state** fire would have constant ROS, flame shape, and energy release, given uniform fuel and environmental conditions.  
- True steady fires are rarely observed in nature because real conditions are too variable.  

### Quasi-Steady Behaviour
- In practice, fires often settle into a state where their behaviour **fluctuates around a mean**.  
- While instantaneous measurements vary, averages over time and space remain consistent.  
- This is what is meant by **quasi-steady fire behaviour**.

---

## The Quasi-Steady Ansatz

In mathematical modelling, an **ansatz** is an assumption or starting point used to simplify a problem.  
The **quasi-steady ansatz** in fire science assumes:

> [!note]  
> *The fire front can be modelled as though it is in a steady state, even though the actual system is unsteady, because the short-term fluctuations average out.*

This ansatz underpins many classical fire spread models, including:
- **[[The Rothermel model|Rothermel’s spread model]] (1972)** – assumes a steady propagation rate for given conditions.  
- **[[McArthur FFDI|McArthur’s Fire Danger Index and spread models]]** – relate steady weather conditions to steady fire behaviour.  

---

## General Form of Quasi-Steady Models

Quasi-steady spread models take the general mathematical form:

![[Eq - Quasi-steady ansatz#^7444b8]]


where $R$ is the **rate of spread**, and the $X_i$ represent environmental and fuel variables (e.g., drought factor, temperature, humidity, wind speed, fuel load).  

If the model satisfies the quasi-steady ansatz, then it is classified as a **quasi-steady spread model**.

---

## Example: McArthur Mark V Forest Fire Model

One of the most famous quasi-steady spread models is **McArthur’s Mark V Forest Fire Model**, which expresses ROS as:

$$
R = f(D, T, H, U, \mu)
$$

where:
- $D$ = Drought Factor (0–10),  
- $T$ = Temperature (°C),  
- $H$ = Relative Humidity (%),  
- $U$ = Wind Speed (km/h),  
- $\mu$ = Fuel Load (tonnes/ha).  

This model was originally distributed as a **fire danger meter** (a circular slide rule), allowing field users to quickly compute fire danger. It was later expressed as an explicit equation:

$$
R = 0.0024 \exp\left( -0.45 + 0.987 \ln(D) - 0.0345 H + 0.0338 T + 0.0234 U \right)
$$

- $R$ = rate of spread (m/min).  
- Inputs are taken at **1500 hours local time**, when fire danger is typically at its peak.  

---

## Worked Example

**Case 1:**  
- $D = 7$,  
- $T = 35^\circ$C,  
- $H = 32\%$,  
- $U = 20$ km/h,  
- $\mu = 5$ t/ha.  

Substituting into the equation:  

$$
R = 0.0024 \exp\left( -0.45 + 0.987 \ln(7) - 0.0345(32) + 0.0338(35) + 0.0234(20) \right)
$$

This gives a **rate of spread** in the range of a few metres per minute.

**Case 2:**  
- $D = 10$,  
- $T = 45^\circ$C,  
- $H = 5\%$,  
- $U = 40$ km/h,  
- $\mu = 20$ t/ha.  

Under these conditions, the calculated **ROS is extremely high**, consistent with catastrophic fire behaviour.

---

## Implications for Fire Modelling

### Advantages
- **Simplifies analysis**: Enables tractable equations for ROS and intensity.  
- **Predictive power**: Captures average fire behaviour, which is most relevant for planning and management.  
- **Operational utility**: The McArthur meters and equations are easy to use in the field.

### Limitations
- **Short-term variability ignored**: Spotting, turbulence, and pulsations are not represented.  
- **Dependence on empirical calibration**: Coefficients are derived from mid-20th century field data.  
- **Climate shift**: Modern extreme conditions may exceed the calibration range.  

---

## Summary

The **quasi-steady fire behaviour concept** recognises that fires fluctuate but often settle into a state where their behaviour is approximately constant on average. The **quasi-steady ansatz** allows models to approximate this behaviour as steady, enabling simplified spread predictions.  

Quasi-steady spread models like **McArthur’s Mark V** provide practical, empirical equations for rate of spread based on key environmental drivers. While powerful, these models must be applied with an awareness of their limitations, especially under today’s extreme fire weather conditions.

$$\boxed{R = f(D, T, H, U, \mu) \quad \text{with inputs: drought, temp, humidity, wind, fuel}}$$
