---
aliases:
tags:
---
# McArthur Forest Fire Danger Index (FFDI)

The **McArthur Forest Fire Danger Index (FFDI)** is Australia’s most widely used measure of bushfire danger. Developed in the 1960s by **Alan G. McArthur**, a forester with the Commonwealth Scientific and Industrial Research Organisation (CSIRO), the index provides a way to **quantify bushfire risk** based on weather and fuel conditions. The FFDI has been central to bushfire management, firefighting operations, and public safety communication in Australia for decades.

---

## Historical Background

- **Pre-FFDI fire danger ratings**: Before McArthur’s work, fire danger assessments relied on more subjective or localised measures that lacked standardisation.
- **McArthur’s contribution**: Using extensive field observations and experiments, McArthur designed **fire danger meters** (sliding card calculators) that related meteorological data to fire behaviour.
- **Legacy**: His Forest Fire Danger Meter (Mark V, 1967) became the foundation for the FFDI. Later versions also included grassland fire danger indices.

The FFDI underpins today’s **Australian Fire Danger Rating System (AFDRS)**, and its thresholds are used to define categories like *Low–Moderate*, *High*, *Extreme*, and *Catastrophic* fire danger.

---

## Conceptual Basis

The FFDI is built on the understanding that **fire behaviour is influenced by weather, fuel, and drought conditions**. McArthur combined these variables into a single index designed to reflect the **potential difficulty of controlling a fire** if one were to start.

The index increases with:
- **Higher temperature**: Fires ignite and spread more readily in hot conditions.
- **Lower relative humidity**: Drier air dries fuels faster, making them more flammable.
- **Stronger winds**: Winds tilt flames forward, pre-heat fuel, and carry embers.
- **Longer drought periods**: Extended dryness reduces fuel moisture content.
- **Drier fuels**: Measured through the **Drought Factor (DF)**, which accounts for recent rainfall and drying trends.

---

## Mathematical Formulation

The McArthur FFDI is calculated using the following empirical equation:

![[Eq - MacArthur FFDI#^03f9f8]]

where:

![[Eq - MacArthur FFDI#^99de3e]]

**Key notes:**
- The exponential form ensures the index grows rapidly under severe conditions.  
- Inputs are taken at **mid-afternoon**, when fire danger typically peaks.  
- The formula is empirical, derived from extensive regression analysis of fire behaviour data.

---

## Fire Danger Categories

Australia classifies FFDI values into operational **fire danger ratings**, used in public warnings:

![[Eq - MacArthur FFDI#^5dcdac]]

These categories underpin **total fire bans**, evacuation advisories, and resource deployment.

---

## The Drought Factor (DF)

A critical part of the formula is the **Drought Factor**, which varies from 0 (saturated fuels) to 10 (extremely dry fuels). DF is calculated from:
- **Keetch–Byram Drought Index (KBDI)**,  
- **rainfall in the past weeks**, and  
- **soil and fuel moisture models**.  

It reflects the **availability of fuel to burn**, not just surface dryness. A high DF means even deep or shaded fuels are highly combustible.

---

## Interpretation and Use

- **Operational Use**: Fire agencies calculate the FFDI daily during fire season. It informs bans on burning, readiness levels, and public warnings.
- **Public Safety**: Warnings are often issued in simple categories rather than raw FFDI numbers, but agencies retain the index for operational planning.
- **Regional Variation**: Thresholds for danger categories may differ slightly between states and territories due to local climate and fuel conditions.

---

## Interactive Panel
Test out the model with the following slider - 

![[McArthur FFDI Slider]]
## Strengths and Limitations

**Strengths:**
- Provides a **quantitative, standardised measure** of fire danger across Australia.
- Simple inputs (temperature, humidity, wind, drought factor) make it operationally practical.
- Empirically validated with decades of fire behaviour data.

**Limitations:**
- Assumes **forest fuels** (a separate Grassland Fire Danger Index exists, but with similar principles).
- Does not account for **fine-scale fuel variability** (fuel loads, structure, curing).
- Lacks explicit treatment of **extreme phenomena** (pyro-convection, ember storms).
- Based on historical climate data from the 1950s–60s — its calibration may underestimate risk under modern **climate change conditions**.

---

## Summary

The **McArthur FFDI** is a cornerstone of Australian bushfire management. By combining weather data with fuel dryness, it provides a **numerical measure of bushfire danger** that guides firefighting, policy, and public safety. While it has limitations, especially under extreme and climate-shifted conditions, it remains one of the most influential and widely applied fire danger indices globally.
