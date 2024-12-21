---
layout: page
title:  "Simple Mediation Model"
subtitle: ""
date:   2024-01-22 22:00:21 +0530
categories: ["Mediation Analysis"]
---


<p>In the <strong>Manipulation-of-Mediation-as-a-Moderator</strong> (MMM) post, I introduced the requirements for supporting mediation hypotheses. Specifically, in order to support that the mediating variable M mediates the relationship from X to Y, we need to rule out the following assumptions:</p>
<ul>
  <li><strong>No Effect on Mediator:</strong> Changes in X don't lead to changes in M.</li>
  <li><strong>No Effect on Outcome:</strong> Changes in M don't affect Y.</li>
  <li><strong>Independent Effects:</strong> Even if X affects M and M affects Y, the influence of M on Y is independent of X's effect on Y.</li>
</ul>
<p>I also explained that statistical mediation analysis cannot support any mediation hypothesis. However, as a basic method of mediation analysis, statistical mediation analysis can provide clues for mediation relationships and direct the experimental testing of mediation. Therefore, I will introduce the three most commonly used models, starting with the simple mediation model.</p>

<h2><strong>Simple Mediation Model</strong></h2>
<p>The simple mediation model, shown below, involves an independent variable X, a mediator variable M, and a dependent variable Y. X predicts both M and Y, and M predicts Y. When we construct and test this model in some data analysis software, we can determine whether there is such a relationship between the three variables and whether M mediates the effect of X on Y.</p>

<img src="{{ '/assets/img/20240123/SM1.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<h2><strong>How to Construct This Model in Mplus?</strong></h2>
<p>To construct a simple mediation model in Mplus, the key is to express the corresponding paths in the model using statements in the Model section, as shown in the following syntax:</p>
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

<p>This code allows us to examine the coefficients and significance for the paths X → M, Y and M → Y, but more importantly, we want to determine whether M mediates the effect of X on Y. To test the mediation effect, we can use the Bootstrap method, which is implemented in Mplus as follows:</p>

<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat;

VARIABLE: NAME ARE X M Y; 
          MISSING=ALL(99); 
          USEVARIABLE ARE X M Y;

ANALYSIS: BOOTSTRAP=2000; ! Use the Bootstrap method for mediation effect testing
            ! Number of bootstrap samples can be set to 1000, 2000, or 5000 based on literature support

MODEL: Y M ON X;
       Y ON M;

MODEL INDIRECT: Y IND M X; ! Mediation path: X → M → Y

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL;
</pre>

<h2><strong>Interpretation of Results:</strong></h2>
<p>After running the above code in Mplus, you will obtain the output file (of the .out type). Key areas to focus on include model fit, path coefficients and significance, as well as mediation effects and their significance.</p>

<ul>
  <li><strong>Model Fit:</strong> Model fit refers to how well the constructed model aligns with the collected data. A good model fit indicates that constructing this model for the data is appropriate and useful for the analysis. Since this is a saturated model, model fit is not necessary to check.</li>
<img src="{{ '/assets/img/20240123/SM2.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
  <li><strong>Path Coefficients and Significance:</strong> The following figure show the non-standardized path coefficients above and the standardized path coefficients below. From the figure, we can see that the coefficient for the path X → Y is 0.173 and significant at the 0.001 level. The coefficient for the path M → Y is 0.180 and significant at the 0.001 level. The coefficient for the path X → M is 0.348 and significant at the 0.001 level. Thus, X significantly predicts both M and Y, and M significantly predicts Y.</li>
<img src="{{ '/assets/img/20240123/SM3.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
  <li><strong>Mediation Effect and Significance:</strong> The following figure shows the non-standardized and standardized mediation effects and their significance. The non-standardized mediation effect is 0.063, and the standardized effect is 0.099. To test the significance of the mediation effect, we use the Bootstrap method. If the 95% confidence interval for the mediation effect does not include 0, the mediation effect is considered significant; otherwise, it is not. From the confidence interval, we see that the non-standardized mediation effect's 95% confidence interval is 0.043–0.082, and the standardized effect's 95% confidence interval is 0.069–0.129, both of which do not include 0, indicating that the mediation effect is significant.</li>
<img src="{{ '/assets/img/20240123/SM4.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
  <li><strong>Model Diagram:</strong> In Mplus, you can open the output file, go to the menu bar, select "Diagram," and click on "View diagram" to obtain the model diagram shown below.</li>
<img src="{{ '/assets/img/20240123/SM5.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
</ul>

<h2><strong>Latent Variable Simple Mediation Model</strong></h2>
<p>In addition to the simple mediation model described above, you may encounter models in the literature that look similar but use latent variables instead of observed variables. Latent variables, which cannot be directly observed, need to be estimated using indicators from observed variables. For example, the scores on each question in a scale are observable, so each question is an observed variable. The latent trait being measured by the scale is not directly observable, but it can be represented by the scale's average or total score.</p>

<p>Below is the Mplus syntax for a latent variable simple mediation model:</p>

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

<p><strong>Note:</strong></p>
<ul>
  <li>1. If there are too many indicators for the latent variables, you can simplify the model by grouping the indicators, which can improve the model fit.</li>
  <li>2. Bootstrap and MLR estimation methods cannot be used simultaneously.</li>
</ul>
