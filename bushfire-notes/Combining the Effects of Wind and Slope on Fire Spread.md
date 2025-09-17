---
aliases:
tags:
---

Wind and slope are two of the most important environmental factors influencing **wildfire spread**. Both tilt the flames forward, enhancing heat transfer to unburned fuel. When wind and slope are aligned (e.g. wind blowing upslope), their effects are straightforward to combine. However, when they are **not aligned**, the problem becomes more complex, and different modelling approaches have been proposed.

---

## Wind and Slope Effects Individually

- **Wind**: Increases rate of spread (ROS) by tilting flames forward and driving convection.  
- **Slope**: A fire burning upslope also spreads faster, as flames are closer to unburned fuel above.  

Classical models treat wind and slope effects differently:

- **McArthur (Mark V model):**  
  McArthur’s spread model does not use explicit wind and slope coefficients. Instead, it includes **wind speed ($U$)**, **temperature ($T$)**, **relative humidity ($H$)**, and **drought factor ($D$)** directly in its exponential equation:  

![[Eq - MacArthur FFDI#^03f9f8]]
  

- **Rothermel (1972 model):**  
  Rothermel explicitly formulated wind and slope effects as multiplicative corrections to the baseline spread rate:  

  $$
  R = \frac{H_{p,0}}{\rho_{be} Q_{ig}} (1 + \phi_w + \phi_s)
  $$  

where:  
- $\phi_w$ = wind coefficient,  
- $\phi_s$ = slope coefficient,  
- $H_{p,0}$ = propagating heat flux in flat, no-wind conditions,  
- $\rho_{be}$ = effective bulk density,  
- $Q_{ig}$ = heat of pre-ignition.  


---

## The Challenge: Non-Aligned Wind and Slope

When wind and slope are aligned, their combined effect is straightforward (multiplicative or additive correction). But if wind and slope act in **different directions**, the situation is less clear:

- There is **no universally accepted method** for combining non-aligned wind and slope effects.  
- Modern fire simulators often use **vector methods**, while older or simpler approaches may use **scalar corrections**.

---

## Scalar vs Vector Methods

### Scalar Methods
- Treat the combined effect as a simple adjustment in the **direction of the wind**.  
- Replace the slope angle with the slope **measured along the wind direction**.  
- Easy to implement, but may oversimplify directional effects.  

**McArthur Scalar Method**:  
Apply the slope correction using the slope angle in the wind’s direction.  

**Rothermel Scalar Method**:  
Use the slope coefficient with the slope sensed in the wind’s direction.  

---

### Vector Methods
- Decompose the **wind-induced rate of spread vector** into two components:  
  - **Upslope component**  
  - **Transverse component**  
- Correct the **upslope component** for slope effects.  
- Recombine the corrected upslope and transverse components into a final **ROS vector**.  

**McArthur Vector Method**:  
- Decompose wind ROS into upslope and transverse parts.  
- Correct upslope part for slope.  
- Add corrected upslope back with transverse to get final ROS.  

**Rothermel Vector Method**:  
- Similar decomposition, but apply slope correction alongside wind correction.  
- Produces a more physically realistic **combined ROS vector**.  

---

## General Framework

1. **Scalar Approach**:  
   $$
   R = R_{\text{wind}} \cdot f(\gamma_{\text{slope,wind}})
   $$  
   where $\gamma_{\text{slope,wind}}$ is the slope angle in the wind direction.  

2. **Vector Approach**:  
   - Start with wind-induced ROS vector.  
   - Correct upslope component for slope.  
   - Recombine with transverse.  

This produces a **directional rate of spread vector**, better suited for modelling complex terrain.

---

## Practical Considerations

- **Modern fire simulators** (e.g. PHOENIX, FARSITE) generally use **vector-based methods** because they better capture real-world interactions between wind and slope.  
- **Simpler indices** (like McArthur’s FFDI or Rothermel’s basic model) rely on **scalar methods** for ease of use.  
- Choice of method depends on the balance between **accuracy** and **operational simplicity**.  

---

## Summary

- Wind and slope both accelerate fire spread, but combining their effects is complex when they are not aligned.  
- **Scalar methods** are simple but can be unrealistic in complex terrain.  
- **Vector methods** are more physically consistent, decomposing and recombining spread components.  
- There is still **no definitive universal model**, though modern fire simulators increasingly adopt **vector combination frameworks**.  
