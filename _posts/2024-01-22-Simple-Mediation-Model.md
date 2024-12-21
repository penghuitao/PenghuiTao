---
layout: page
title:  "Simple Mediation Model"
subtitle: ""
date:   2024-01-22 22:00:21 +0530
categories: ["Mediation Analysis"]
---


<p>In the Manipulation-of-Mediation-as-a-Moderator, I introduced the requirements for supporting mediation hypotheses:</p>
<ul>
  <li><strong>No Effect on Mediator:</strong> Changes in X don't lead to changes in M.</li>
  <li><strong>No Effect on Outcome:</strong> Changes in M don't affect Y.</li>
  <li><strong>Independent Effects:</strong> Even if X affects M and M affects Y, the influence of M on Y is independent of X's effect on Y.</li>
</ul>
<p>I also explained that statistical mediation analysis cannot support any mediation hypothesis. However, as a basic method of mediation analysis, statistical mediation analysis can provide clues for mediation relationships and direct the experimental testing of mediation. Therefore, I will introduce the three most commonly used models, starting with the simple mediation model.</p>

<h3>Simple Mediation Model</h3>
<p>The simple mediation model involves an independent variable X, a mediator variable M, and a dependent variable Y. X predicts both M and Y, and M predicts Y. When we construct and test this model in some data analysis software, we can determine whether there is such a relationship between the three variables and whether M mediates the effect of X on Y.</p>

<h4>How to Construct This Model in Mplus?</h4>
<pre>
TITLE: MEDIATION MODEL;       ! TITLE is optional and can be used to label the content or purpose of the statement

DATA: FILE IS 12.dat; ! Data file name and source
VARIABLE: NAME ARE X M Y; ! Variables, must match the number and order of variables in the data file
          MISSING=ALL(99); ! Define missing values, indicating that Mplus treats 99 as a missing value
          USEVARIABLE ARE X M Y; ! Variables to be used in the analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, can choose other methods like ML based on data characteristics

MODEL: Y M ON X; ! Path from X to M and Y
       Y ON M; ! Path from M to Y

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, and confidence intervals
</pre>

<h4>Interpretation of Results:</h4>
<ul>
  <li><strong>Model Fit:</strong> A good model fit indicates that constructing this model for the data is appropriate and useful for the analysis.</li>
  <li><strong>Path Coefficients and Significance:</strong> Examining the coefficients for the paths X → M, Y and M → Y to determine their significance.</li>
  <li><strong>Mediation Effect and Significance:</strong> Using the Bootstrap method to test the mediation effect and its significance.</li>
  <li><strong>Model Diagram:</strong> Mplus provides a model diagram after running the analysis, which visualizes the relationships between variables.</li>
</ul>

<h3>Latent Variable Simple Mediation Model</h3>
<p>In some cases, instead of observed variables, latent variables may be used to represent constructs. Below is the Mplus syntax for a latent variable simple mediation model:</p>
<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat;

VARIABLE: NAME ARE X1-X4 M1-M4 Y1-Y4; ! Observed variables/indicators for latent variables
          MISSING=ALL(99); USEVARIABLE ARE X1-X4 M1-M4 Y1-Y4;

ANALYSIS: ESTIMATOR=MLR; ! Estimation method

MODEL: A BY X1-X4; 
       B BY M1-M4; 
       C BY Y1-Y4; ! Define latent variables using observed indicators
       C B ON A; ! Path from A to B and C
       C ON B; ! Path from B to C

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, and confidence intervals
</pre>
