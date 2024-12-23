---
layout: page
title:  "Parallel Mediation Model"
subtitle: ""
date:   2024-02-16 22:00:21 +0530
categories: ["Mediation Analysis"]
---

<p>In addition to the simple mediation model, one of the common models with two mediator variables is the parallel mediation model. In this model, it is assumed that there is no temporal order between the two mediator variables, as shown in the figure below. <b>As before, I want to emphasize that for mediation tests, I strongly recommend that you use experimental designs rather than statistical mediation tests</b>.</p>

<img src="{{ '/assets/img/20240123/PM1.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<h2><strong>Observed Variable Parallel Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL; 

DATA: FILE IS 12.dat;      ! Data file name and source
                            ! If the data file and the code are in the same folder, just write the name
                            ! If they are in different folders, specify the path to the data file

VARIABLE: NAME ARE X M1 M2 Y; ! Variables, must match the order and quantity of variables in the data file
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: Y M1 M2 ON X; ! Paths from X to M1, M2, and Y
       Y ON M1 M2; ! Paths from M1 and M2 to Y

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Latent Variable Parallel Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variables used in the analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: A BY X1-X4; 
       B BY A1-A4;
       C BY B1-B4;
       D BY Y1-Y4; ! Define four latent variables using BY
       D B C ON A; ! Paths from A to B, C, and D
       D ON B C; ! Paths from B and C to D

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Mediation Effect Testing</strong></h2>
<p>For the observed variable model, the syntax for testing the mediation effect using the Bootstrap method is as follows. The same approach applies to latent variable models, where the BOOTSTRAP method is used in the ANALYSIS section and the mediation paths to be tested are specified in the MODEL INDIRECT section.</p>

<pre>
TITLE: MEDIATION MODEL; 

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: BOOTSTRAP=2000; ! Use Bootstrap method for mediation effect testing, sample size can be 1000/2000/5000 based on literature support

MODEL: Y M1 M2 ON X; ! Paths from X to M1, M2, and Y
       Y ON M1 M2; ! Paths from M1 and M2 to Y

MODEL INDIRECT: Y IND M1 X; ! Mediation effect of X -> M1 -> Y
                Y IND M2 X; ! Mediation effect of X -> M2 -> Y
                Y IND X;  ! Total indirect effect of X on Y

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Interpretation of Results</strong></h2>
<ul>
  <li><strong>Model Fit:</strong> Model fit can be assessed by selecting fit indices and standards from the literature to evaluate the adequacy of the model's fit to the data.</li>

<img src="{{ '/assets/img/20240123/PM3.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

  <li><strong>Path Coefficients and Significance:</strong> The path coefficients and their significance should be reviewed to determine the strength and significance of the relationships.</li>

<img src="{{ '/assets/img/20240123/PM4.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

  <li><strong>Mediation Effect and Significance:</strong> The mediation effects and their significance should be examined to determine whether mediation exists and whether the effects are statistically significant.</li>

<img src="{{ '/assets/img/20240123/PM5.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

</ul>
<p>If you want to learn more, I recommend reading the following articles, all of which use the parallel mediation model:
<br>Kane, L., & Ashbaugh, A. R. (2017). Simple and parallel mediation: {A} tutorial exploring anxiety sensitivity, sensation seeking, and gender. <i>The Quantitative Methods for Psychology, 13</i>(3), 148–165. <a href="https://doi.org/10.20982/tqmp.13.3.p148">https://doi.org/10.20982/tqmp.13.3.p148</a>
<br>Ranjan, S., & Dash, S. (2022). Impact of internal corporate social responsibility: a parallel mediation analysis. <i>Personnel Review, 53</i>(1), 119–135. <a href="https://doi.org/10.1108/pr-05-2020-0354">https://doi.org/10.1108/pr-05-2020-0354</a>
<br>Shafique, I., Qammar, A., Kalyar, M. N., Ahmad, B., & Mushtaq, A. (2020). Workplace ostracism and deviant behaviour among nurses: a parallel mediation model. <i>Journal of Asia Business Studies, 15</i>(1), 50–71. <a href="https://doi.org/10.1108/jabs-03-2020-0096">https://doi.org/10.1108/jabs-03-2020-0096</a>
<br>If you have any questions, feel free to email me.</p>
