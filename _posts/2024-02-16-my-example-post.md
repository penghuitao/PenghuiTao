---
layout: page
title:  "Measurement Invariance: Longitudinal Confirmatory Factor Analysis"
subtitle: "How to implement it in Mplus"
date:   2024-02-16 21:21:21 +0530
categories: ["longitudinal analysis"]
---

## Introduction  
In contemporary psychological research, longitudinal studies are increasingly favored by scholars due to their many advantages that cross-sectional studies cannot match. Longitudinal research is an important research design for exploring changes in individuals or groups over time. By tracking data from the same sample at multiple time points, researchers can observe and analyze the development and trajectories of various factors such as behavior, psychological states, and physiological changes. This research design plays an irreplaceable role in understanding psychological development, educational outcomes, and the long-term effects of health interventions. Additionally, longitudinal designs help researchers reveal causal relationships.

However, a major challenge in longitudinal research is ensuring that the measurement tools used maintain measurement invariance across the entire study period. Longitudinal measurement invariance refers to whether the same measurement tool consistently measures the same psychological construct or behavioral standard at different time points. Verifying this invariance ensures the reliability of the research results, meaning that the observed changes are due to actual changes in the subjects being studied, not inconsistencies in the measurement tool. In this chapter, I will introduce Longitudinal Confirmatory Factor Analysis (LCFA) to help you test for longitudinal measurement invariance.

## Steps for Longitudinal Confirmatory Factor Analysis  
The analysis steps for longitudinal measurement invariance are similar to those for multi-group measurement invariance, but there are some differences in model specification. Specifically, in a standard CFA model, we generally do not allow error covariances between items, but in longitudinal measurement, the same item is used multiple times. Therefore, in a longitudinal invariance test model, error covariances for the same item at different time points are allowed. Longitudinal measurement invariance can be distinguished into four levels, from less restrictive to more restrictive, and involves four steps:

### 1. Configural Invariance  
This is the most basic test, ensuring that the scale measures the same factor structure at all time points. In this stage, no constraints are placed on factor loadings or intercepts, and the only requirement is that the structural model is consistent across time points.

### 2. Weak Invariance  
Under the assumption of the same structure, weak invariance refers to constraining the factor loadings across time points to be equal. This step ensures that the items on the scale maintain consistent relationships with the corresponding latent variables, thus ensuring the same interpretability across different time points.

### 3. Strong Invariance  
With weak invariance in place, strong invariance constrains the intercepts of the measurement models to be equal across time points. The confirmation of strong invariance is a prerequisite for comparing the latent variable means across time points.

### 4. Strict Invariance  
With strong invariance in place, strict invariance constrains the measurement error variances to be equal across time points.

These four models are built step by step, with more stringent restrictions being applied in each subsequent step. There are generally two strategies for comparing models. The first is the change in the chi-square (χ²) value between models. If the chi-square value changes significantly after applying the restrictions, the corresponding invariance hypothesis is rejected. However, chi-square values are sensitive to sample size and often tend to be significant in practical analysis. Therefore, in practice, we use ΔRMSEA and ΔCFI as the primary criteria, with ΔRMSEA < 0.015 and ΔCFI < 0.01 as the standards for accepting the invariance hypothesis (van de Schoot et al., 2012). ΔRMSEA = |RMSEA of the previous model - RMSEA of the next model|, and ΔCFI = |CFI of the previous model - CFI of the next model| (Meade et al., 2008). Based on considerations of statistical power and Type I error rates, it is recommended to adopt stricter standards (CFI < 0.002) for determining invariance.

## How to Implement  
### Mplus Syntax

**Step 1: Configural Invariance**  
```mplus
TITLE: Longitudinal Measurement Invariance Test;  
DATA: FILE IS longitudinaldata.dat;  
VARIABLE: Names are t1y1-t1y5 t2y1-t2y5;  
ANALYSIS: TYPE=GENERAL; ESTIMATOR=ML;  
MODEL:  
T1y by t1y1-t1y5;  
T2y by t2y1-t2y5;  
t1y1-t1y5 pwith t2y1-t2y5;  
