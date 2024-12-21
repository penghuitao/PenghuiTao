---
layout: page
title:  "Measurement Invariance: Longitudinal Confirmatory Factor Analysis"
subtitle: ""
date:   2024-07-30 21:21:21 +0530
categories: ["Longitudinal Analysis"]
---

<h2><strong>Preface</strong></h2>
<p>In psychological research, longitudinal studies are increasingly favored due to their many advantages over cross-sectional studies. Longitudinal research is an essential design for exploring changes in individuals or groups over time. By tracking data from the same sample at multiple time points, researchers can observe and analyze the development of behavior, psychological states, and physiological changes. In this article, I will explain how to implement Longitudinal Confirmatory Factor Analysis (LCFA) to test longitudinal measurement invariance using Mplus. The provided Mplus syntax will help you ensure that your measurement tools maintain consistency over time, thereby validating the reliability of your study's results.</p>

<h2><strong>What is Longitudinal Measurement Invariance?</strong></h2>
<p>Longitudinal measurement invariance refers to the consistency of a measurement tool across different time points. Specifically, it tests whether the tool measures the same psychological construct or behavioral standard in the same way over time. Verifying this invariance ensures that any observed changes in the study subjects are not due to inconsistencies in the measurement tool but rather reflect actual changes in the subjects themselves. This process is critical for ensuring that longitudinal research findings are reliable and valid.</p>

<h2><strong>Steps for Testing Longitudinal Measurement Invariance</strong></h2>
<p>In longitudinal analysis, the steps for testing measurement invariance are similar to those in multi-group analysis, with some differences in model specification. The four key levels of measurement invariance are:</p>
<ol>
  <li><strong>Configural Invariance</strong>: This step ensures that the same factor structure is measured across all time points, without imposing any constraints on factor loadings or intercepts.</li>
  <li><strong>Weak Invariance</strong>: This step constrains factor loadings across time points to ensure that the items maintain consistent relationships with the latent variables at different time points.</li>
  <li><strong>Strong Invariance</strong>: This step constrains the intercepts to be equal across time points, allowing for comparisons of latent variable means across time.</li>
  <li><strong>Strict Invariance</strong>: This step constrains the measurement error variances to be equal across time points, ensuring that error variance remains constant across different measurements.</li>
</ol>

<p>The process of testing these invariances proceeds sequentially, with each step applying more restrictive constraints. If you observe significant changes in fit statistics (such as chi-square values), the corresponding invariance hypothesis may need to be rejected. However, in practical applications, we recommend using changes in RMSEA (ΔRMSEA) and CFI (ΔCFI) as the primary criteria for determining measurement invariance. Acceptable thresholds for ΔRMSEA are < 0.015 and for ΔCFI < 0.01 (van de Schoot et al., 2012).</p>

<h2><strong>Implementing Longitudinal Confirmatory Factor Analysis in Mplus</strong></h2>
<p>Here is how you can implement LCFA using Mplus syntax:</p>

<h3><strong>Step 1: Configural Invariance</strong></h3>
<Pre>
TITLE: Longitudinal Measurement Invariance Test;
!The title can be left blank.
DATA: FILE IS longitudinaldata.dat;
!Indicate the data file, which can be a dat or csv file. When the code and data are in the same folder, the path where the data is located can be omitted.
VARIABLE: Names are t1y1-t1y5 t2y1-t2y5;  
!All variable names t1y1-t1y5 in the data file are the five entries for time T1, and t2y1-t2y5 are the five entries for time T2.
ANALYSIS: TYPE=GENERAL; ESTIMATOR=ML;  
!Usually, default settings are sufficient.
MODEL:  
T1y by t1y1-t1y5;  
T2y by t2y1-t2y5;  
t1y1-t1y5 pwith t2y1-t2y5;  
!Set the variance correlation of different time errors for the same item.</Pre>

<p>This sets up the model where errors for the same items at different time points are allowed to correlate. This is necessary for longitudinal analysis where items are measured repeatedly over time.</p> 
<h3><strong>Step 2: Weak Invariance</strong></h3> 
<Pre>
T1y by t1y1-t1y5(1-5);  
T2y by t2y1-t2y5(1-5);  
t1y1-t1y5 pwith t2y1-t2y5;</Pre>

<p>By labeling factor loadings with the same numbers across time points, you ensure that the relationships between the items and the latent variables remain consistent across time.</p> 
<h3><strong>Step 3: Strong Invariance</strong></h3>
<Pre>
[t1y1-t1y5](6-10);  
[t2y1-t2y5](6-10);  
[T2y*];</Pre>

<p>This step constrains the intercepts to be equal across time points, which is necessary for comparing the means of latent variables over time.</p> 
<h3><strong>Step 4: Strict Invariance</strong></h3> 
<Pre>
t1y1-t1y5(11-15);  
t2y1-t2y5(11-15); </Pre> 

<p>This step ensures that the measurement error variances are the same across time points, completing the strict invariance testing.</p> 
<p>These four Mplus syntax steps should be run in sequence to obtain four result files. You will then summarize fit statistics (e.g., χ², RMSEA, CFI) and compute ΔRMSEA and ΔCFI to determine the invariance of your model.</p> 
<h2><strong>Further Reading</strong></h2> 
<p>For more detailed information on measurement invariance, I recommend the following articles:
<br>Meade, A. W., Johnson, E. C., & Braddy, P. W. (2008). Power and sensitivity of alternative fit indices in tests of measurement invariance. <i>Journal of Applied Psychology, 93</i>(3), 568-592. <a href="https://doi.org/10.1037/0021-9010.93.3.568">https://doi.org/10.1037/0021-9010.93.3.568</a>
<br>van de Schoot, R., Lugtig, P., & Hox, J. (2012). A checklist for testing measurement invariance. <i>European Journal of Developmental Psychology, 9</i>(4), 486–492. <a href="https://doi.org/10.1080/17405629.2012.686740">https://doi.org/10.1080/17405629.2012.686740</a></p>
<p>That concludes this blog post. If you have any questions, feel free to reach out!</p>
