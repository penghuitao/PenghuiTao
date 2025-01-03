---
layout: page
title:  "Chain Mediation Model"
subtitle: ""
date:   2024-03-10 22:00:21 +0530
categories: ["Mediation Analysis"]
---


<p>In my last blog, I introduced the parallel mediator model. In addition to the parallel mediation model, another common model with two mediator variables is the chain mediation model. In this model, there is a temporal sequence between the two mediator variables. As shown in the figure below, the mediator variable M1 comes first, followed by M2. <b>In addition, I'd like to emphasize that if you want to provide a strong mediation test, I suggest you use an experimental design</b>.</p>

<img src="{{ '/assets/img/20240123/CM1.jpg' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<h2><strong>Observed Variable Chain Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: Y M1 M2 ON X; ! Paths from X to M1, M2, and Y
       Y ON M1 M2; ! Paths from M1 and M2 to Y
       M2 ON M1; ! Path from M1 to M2

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Latent Variable Chain Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variables used in this analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: A BY X1-X4; 
       B BY A1-A4;
       C BY B1-B4;
       D BY Y1-Y4; ! Define four latent variables using BY
       D B C ON A; ! Paths from A to B, C, and D
       D ON B C; ! Paths from B and C to D
       C ON B; ! Path from B to C

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Mediation Effect Testing</strong></h2>
<h3>The Bootstrap method is used to test the mediation effect.</h3>

<p>Using the <code>MODEL CONSTRAINT</code> command, we can define the mediation effects, but this method only provides unstandardized values after running. Below is an example:</p>

<pre>
TITLE: MEDIATION MODEL; 

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
MISSING=ALL(99); ! Define missing values
USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: BOOTSTRAP=2000; ! Use Bootstrap method for mediation effect testing, sample size can be 1000/2000/5000 based on literature support

MODEL: Y ON X (cdash); ! Path from X to Y named as c'
Y ON M1 (b1); ! Path from M1 to Y named as b1
Y ON M2 (b2); ! Path from M2 to Y named as b2
M1 ON X (a1); ! Path from X to M1 named as a1
M2 ON X (a2); ! Path from X to M2 named as a2
M2 ON M1(d); ! Path from M1 to M2 named as d

MODEL CONSTRAINT:
NEW(a1b1 a2b2 a1db2 TOTALIND TOTAL); ! Define new coefficients
a1b1 = a1*b1; ! Indirect effect of X through M1 on Y
a2b2 = a2*b2; ! Indirect effect of X through M2 on Y
a1db2 = a1*d*b2; ! Indirect effect of X through M1 and M2 on Y
TOTALIND = a1*b1 + a2*b2 + a1*d*b2; ! Total indirect effect of X on Y
TOTAL = a1*b1 + a2*b2 + a1*d*b2 + cdash; ! Total effect of X on Y = indirect + direct

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h3>Using the MODEL INDIRECT command, this method generates both unstandardized and standardized values and is simpler. It is recommended to use this method.</h3>

<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source
VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
           MISSING=ALL(99); ! Define missing values
           USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: BOOTSTRAP=2000; ! Use Bootstrap method for mediation effect testing, sample size can be 1000/2000/5000 based on literature support

MODEL: Y ON X; 
       Y ON M1; 
       Y ON M2; 
       M1 ON X; 
       M2 ON X; 
       M2 ON M1; 
       
MODEL INDIRECT: Y IND X; ! Computes indirect effects of X through M1 on Y
                               ! Computes indirect effects of X through M2 on Y
                               ! Computes indirect effects of X through both M1 and M2 on Y
                               ! Computes total indirect effect

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>
<h2><strong>Interpretation of Results</strong></h2>
<ul>
  <li><strong>Model Fit:</strong> Model fit can be assessed by selecting fit indices and standards from the literature to evaluate the adequacy of the model's fit to the data.</li>

<img src="{{ '/assets/img/20240123/CM2.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

  <li><strong>Path Coefficients and Significance:</strong> The path coefficients and their significance should be reviewed to determine the strength and significance of the relationships.</li>

<img src="{{ '/assets/img/20240123/CM3.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

  <li><strong>Mediation Effect and Significance:</strong> The mediation effects and their significance should be examined to determine whether mediation exists and whether the effects are statistically significant.</li>

<img src="{{ '/assets/img/20240123/CM4.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

</ul>
<p>If you want to learn more, I recommend reading the following articles, all of which use the chain mediation model:
<br>Chen, K., Huang, L., & Ye, Y. (2022). Research on the relationship between wellness tourism experiencescape and revisit intention: a chain mediation model. <i>International Journal of Contemporary Hospitality Management, 35</i>(3), 893–918. <a href="https://doi.org/10.1108/ijchm-01-2022-0050">https://doi.org/10.1108/ijchm-01-2022-0050</a>
<br>Gori, A., Topino, E., & Di Fabio, A. (2020). The protective role of life satisfaction, coping strategies and defense mechanisms on perceived stress due to COVID-19 emergency: A chained mediation model. <i>PLoS ONE, 15</i>(11), e0242402. <a href="https://doi.org/10.1371/journal.pone.0242402">https://doi.org/10.1371/journal.pone.02424020</a>
<br>Wang, C., Chudzicka-Czupała, A., Tee, M. L., Núñez, M. I. L., Tripp, C., Fardin, M. A., Habib, H. A., Tran, B. X., Adamus, K., Anlacan, J., García, M. E. A., Grabowski, D., Hussain, S., Hoang, M. T., Hetnał, M., Le, X. T., Ma, W., Pham, H. Q., Reyes, P. W. C., . . . Sears, S. F. (2021). A chain mediation model on COVID-19 symptoms and mental health outcomes in Americans, Asians and Europeans. <i>Scientific Reports, 11</i>(1). <a href="https://doi.org/10.1038/s41598-021-85943-7">https://doi.org/10.1038/s41598-021-85943-7</a>
<br>If you have any questions, feel free to email me.</p>
