---
aliases:
tags:
---

![[Def - McArthur’s Mark V Forest Fire Model]]

The **McArthur Mark V Forest Fire Model** is one of the foundational models in Australian fire science, developed by **Alan G. McArthur** of the CSIRO in 1967. It provides a practical way to estimate the **rate of spread (ROS)** and **fire danger** in eucalyptus forests, forming the basis of the **[[McArthur FFDI|Forest Fire Danger Index]] (FFDI)** that remains central to bushfire management today.

---

## Historical Background

- **Early fire danger meters**: McArthur produced several fire danger meters (Mark I–IV) through the 1960s, based on extensive field studies and observations.  
- **Mark V release (1967)**: This was the most refined version, distributed as a **circular slide rule** that allowed quick field calculations of fire danger.  
- **Enduring legacy**: The model’s empirical basis and simplicity ensured its adoption by firefighting agencies, and its mathematical form continues to inform operational systems and research.

---

## Conceptual Basis

The Mark V model relies on the **quasi-steady fire behaviour assumption**:  

> For a given set of weather and fuel conditions, the forward spread of a fire can be treated as approximately steady.

Accordingly, the **rate of spread ($R$)** is expressed as a function of key variables:

![[Eq - McArthur’s Mark V Forest Fire Model#^478fc6]]

where:  

- $D$ = **Drought Factor** (0–10), measuring long-term fuel dryness,  
- $T$ = **Air Temperature** (°C), taken at 1500 hours local time,  
- $H$ = **Relative Humidity** (%), also at 1500 hours,  
- $U$ = **Wind Speed** (km/h), measured at 10 m height,  
- $\mu$ = **Fuel Load** (t/ha), representing the available surface litter.  

This formulation recognises that **fire spread is driven by both weather and fuel availability**.

---

## Explicit Equation

From the Mark V model, the function $f$ is expressed in exponential form as:

![[Eq - McArthur’s Mark V Forest Fire Model#^fa46fe]]


- $R$ = rate of spread (m/min),  
- $D$, $T$, $H$, $U$ as defined above.  

Here, $\mu$ (fuel load) is not explicit in the equation, but it is embedded in the model’s calibration for typical dry eucalyptus forests (fuel loads around 5–20 t/ha).

---

## Worked Examples

**Example 1: Moderate conditions**  
- $D = 7$, $T = 35^\circ$C, $H = 32\%$, $U = 20$ km/h.  

$$
R = 0.0024 \exp \Big( -0.45 + 0.987 \ln(7) - 0.0345(32) + 0.0338(35) + 0.0234(20) \Big)
$$  

$\Rightarrow R \approx 2.5$ m/min (very high fire danger).

---

**Example 2: Extreme conditions**  
- $D = 10$, $T = 45^\circ$C, $H = 5\%$, $U = 40$ km/h.  

$$
R = 0.0024 \exp \Big( -0.45 + 0.987 \ln(10) - 0.0345(5) + 0.0338(45) + 0.0234(40) \Big)
$$  

$\Rightarrow R \approx 12$–15 m/min (catastrophic fire danger).  

---

## Interpretation

- **Temperature ($T$)**: Higher temperatures dry fuels and increase ignition probability.  
- **Relative Humidity ($H$)**: Lower humidity accelerates fuel drying and increases spread.  
- **Wind Speed ($U$)**: Strong winds tilt flames forward and drive rapid spread.  
- **Drought Factor ($D$)**: Represents the long-term availability of dry fuel.  
- **Fuel Load ($\mu$)**: Embedded in calibration; heavier fuel beds burn longer and produce higher intensities.  

---

## Interactive Panel

![[McArthur’s Mark V Forest Fire Model Slider]]
## Strengths and Limitations

**Strengths**  
- Provides a **simple operational equation** linking fire spread to measurable weather inputs.  
- Empirically grounded in **Australian eucalypt forest conditions**.  
- Integral to the **FFDI and national fire danger rating system**.  

**Limitations**  
- Fuel load $\mu$ is not explicitly represented, limiting flexibility across ecosystems.  
- Calibrated under 1960s conditions; may **underpredict extreme modern fires**.  
- Assumes quasi-steady behaviour, ignoring spotting, turbulence, or crown fires.  

---

## Summary

McArthur’s Mark V Forest Fire Model remains a cornerstone of Australian bushfire science. By formulating the rate of spread as a function of **drought, temperature, humidity, wind, and fuel**, it provides an operationally useful tool to predict fire behaviour.  

Its empirical equation is simple yet powerful, though modern applications must account for its assumptions and limitations under changing climate conditions.

$$\boxed{R = f(D, T, H, U, \mu)}$$  
$$\boxed{R = 0.0024 \, \exp \Big( -0.45 + 0.987 \ln(D) - 0.0345H + 0.0338T + 0.0234U \Big)}$$
