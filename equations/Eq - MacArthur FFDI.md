---
aliases:
tags:
  - mathematics/equations/mathematical-modelling
  - mathematics/equations/mathematical-modelling/bushfire-models
---

### Core Formula
$$
\text{FFDI} = 2 \times \exp \left[ -0.45 + 0.987 \ln(\text{DF}) - 0.0345H + 0.0338T + 0.0234V \right]
$$  

^03f9f8

Empirical equation combining drought, humidity, temperature, and wind.

---

### Fire Danger Categories

| **FFDI Value** | **Rating**            | **Description**                                                                                         |
| -------------- | --------------------- | ------------------------------------------------------------------------------------------------------- |
| 0 – 11         | Low–Moderate          | Fires can be controlled easily; little risk.                                                            |
| 12 – 24        | High                  | Fires may be difficult to control in some areas.                                                        |
| 25 – 49        | Very High             | Fires will be difficult to control; expect fast spread and spotting.                                    |
| 50 – 74        | Severe                | Fires will spread rapidly and be dangerous; firefighters may struggle.                                  |
| 75 – 99        | Extreme               | Fires will be uncontrollable and very fast-moving; direct attack often unsafe.                          |
| 100+           | Catastrophic/Code Red | Fires are uncontrollable, unpredictable, and extremely dangerous. Survival may depend on leaving early. |

^5dcdac

---

### Drought Factor (DF)
The drought factor ($0$–$10$) reflects fuel availability to burn, influenced by:  
- recent rainfall,  
- soil moisture,  
- drought indices (e.g., KBDI).  

High DF = fuels are critically dry.

---

> [!neutral] **Symbol Glossary**  
> - $\text{FFDI}$ — Forest Fire Danger Index (dimensionless)  
> - $\text{DF}$ — Drought Factor (0–10, fuel dryness)  
> - $H$ — Relative Humidity (%)  
> - $T$ — Air Temperature (°C)  
> - $V$ — Wind Speed (km/h at 10 m height, 1500 hrs)  
> - $\ln$ — Natural logarithm  
> - $\exp$ — Exponential function

^99de3e

