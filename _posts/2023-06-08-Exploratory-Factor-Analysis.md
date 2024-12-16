---
layout: page
title:  "Exploratory Factor Analysis"
subtitle: ""
date:   2023-06-08 21:21:21 +0530
categories: ["Factor Analysis"]
---


<h2><strong>What is Exploratory Factor Analysis (EFA)?</strong></h2>
<p>Exploratory Factor Analysis (EFA) primarily addresses the question: can we extract a few meaningful new dimensions or latent variables from the complex and extensive raw data? That is, EFA seeks to uncover and identify the underlying structure behind the raw data. In EFA, researchers do not have prior theoretical assumptions about the factors they might obtain. Therefore, EFA is usually applied in the initial stages of research. Its main goal is to extract a few latent factors and determine the underlying structure of the original data.</p>

<h2><strong>What Type of Data is Suitable for EFA?</strong></h2>
<p>The goal of factor analysis is to simplify data or uncover the basic data structure. Therefore, the assumptions for conducting factor analysis include both theoretical and statistical assumptions:</p>
<ul>
  <li><strong>Theoretical assumption</strong>: There is some latent structure within the selected set of variables.</li>
  <li><strong>Statistical assumption</strong>: The observed variables should be strongly correlated. If the correlations between the variables are low, they cannot share common factors.</li>
</ul>
<p>SPSS provides the following statistics to assess whether the data is suitable for EFA:</p>
<ul>
  <li><strong>Sample size</strong>: The sample size should be at least five times the number of variables to be analyzed, or preferably ten times or more.</li>
  <li><strong>Correlation matrix of variables</strong>: If most correlations are less than 0.3, the data is not suitable for EFA.</li>
  <li><strong>KMO Measure</strong>: A KMO value close to 1 indicates suitability for EFA.</li>
  <li><strong>Bartlett's Test of Sphericity</strong>: A significant result suggests suitability for EFA.</li>
</ul>

<h2><strong>What Methods Are Used to Extract Common Factors in EFA?</strong></h2>
<p>Factor analysis methods include:</p>
<ul>
  <li><strong>Principal Component Analysis (PCA)</strong>: A method for reducing variables, not a true factor analysis method.</li>
  <li><strong>Unweighted Least Squares (ULS)</strong></li>
  <li><strong>Generalized Least Squares (GLS)</strong></li>
  <li><strong>Maximum Likelihood (ML)</strong></li>
  <li><strong>Principal Axis Factoring (PAF)</strong></li>
  <li><strong>Alpha Factoring</strong></li>
  <li><strong>Image Analysis</strong></li>
</ul>
<p>For normal data, Maximum Likelihood is recommended; for non-normal data, Principal Axis Factoring is preferred.</p>

<h3>How Many Factors Should Be Extracted?</h3>
<p>There is no exact method to determine the number of factors. Criteria include:</p>
<ul>
  <li><strong>Eigenvalue Criterion</strong>: Extract factors with eigenvalues > 1.</li>
  <li><strong>Scree Plot Criterion</strong>: Examine the plot of eigenvalues against factor number.</li>
  <li><strong>Cumulative Variance Explained Criterion</strong>: Typically, a cumulative variance of 60% or more is selected.</li>
  <li><strong>Velicer's MAP Test</strong></li>
</ul>

<h3>How to Interpret the Extracted Common Factors?</h3>
<p>Factor rotation is used to clarify the factor structure, making the relationships more distinct and easier to interpret. There are two types of rotation:</p>
<ul>
  <li><strong>Orthogonal Rotation</strong>: Assumes factors are uncorrelated.</li>
  <li><strong>Oblique Rotation</strong>: Assumes factors can be correlated.</li>
</ul>
<p>Oblique rotation is often preferred when factors are correlated. Each factor should have at least 3-5 variables with a factor loading of 0.30 or higher.</p>

<h2><strong>How to Implement EFA in Mplus</strong></h2>
<pre>
TITLE: EFA; ! The title of the analysis

DATA: FILE IS aq.dat; ! Data file

VARIABLE: NAMES = aq1-aq31; ! Names of the variables
          MISSING = ALL(99); ! Define missing values
          USEVARIABLES = aq1-aq31; ! Variables to be used

ANALYSIS: ROTATION = GEOMIN (oblique); ! Factor rotation method (GEOMIN is the default, other options are available)
          ESTIMATOR = MLR; ! Method to extract common factors
          TYPE = EFA 1 4; ! Define the number of factors to extract (from 1 to 4). Use identical numbers if only extracting a specific number.

OUTPUT: STANDARDIZED MOD; ! Output standardized values and modification indices

PLOT: TYPE IS PLOT2; ! Request scree plot
</pre>
