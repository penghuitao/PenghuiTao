---
layout: page
title:  "Exploratory Structural Equation Modeling"
subtitle: ""
date:   2023-11-03 21:21:21 +0530
categories: ["Factor Analysis"]
---


<h3>If You Just Want to Learn How to Use ESEM</h3>
<p>I strongly recommend that you watch this video: <a href="https://www.youtube.com/watch?v=yK_myOYNi-o">https://www.youtube.com/watch?v=yK_myOYNi-o</a><br>
It will quickly help you get started with ESEM.</p>
<p>Additionally, I highly suggest you read this paper:  
Van Zyl, L. E., & Klooster, P. M. T. (2022). Exploratory Structural Equation Modeling: Practical guidelines and tutorial with a convenient online tool for MPlus. <i>Frontiers in Psychiatry, 12.</i> <a href="https://doi.org/10.3389/fpsyt.2021.795672">https://doi.org/10.3389/fpsyt.2021.795672</a></p>

<h3>What is ESEM?</h3>
<p>Traditional EFA is mainly used to explore the factor structure of a measurement tool or a concept, while CFA is used to validate an existing theoretical factor structure and provide a measurement model for latent variables. ESEM combines the functionalities of both EFA and CFA, including: exploring factor structures, validating factor structures, and analyzing relationships between latent variables. It is important to note that ESEM (Exploratory Structural Equation Modeling) is not the same as EFA; ESEM is a combination of EFA and SEM. The introduction of ESEM primarily aims to address the strong assumptions in the measurement model of SEM.</p>

<p>In traditional SEM, the measurement model consists of multiple CFA models, each assuming no cross-loadings. However, this assumption is often too strong and does not hold in many cases, where a single indicator may have significant loadings on multiple factors. The traditional view is to remove such indicators before conducting SEM, but the current view is that researchers should not focus too much on the specification of the measurement model, but rather shift the focus to the structural model, maximizing the precision of estimating structural coefficients.</p>

<p>It is also important to note that the measurement model in ESEM is not purely EFA because the number of factors must be specified in advance, but the relationships between factors and indicators are not predefined. In summary, ESEM has two key benefits: it avoids mis-specification of the measurement model and provides more precise estimates of structural coefficients. In practical research, it is recommended that researchers compare the results of ESEM and SEM to determine if they yield different conclusions.</p>

<h3>ESEM Factor Analysis Steps</h3>
<ol>
  <li>Model specification</li>
  <li>Factor rotation</li>
  <li>Parameter estimation</li>
  <li>Model evaluation or comparison</li>
  <li>Interpretation of the best-fitting model</li>
</ol>

<h3>How to Implement ESEM in Mplus</h3>
<p>Please note the example code provided below, which includes three cases. However, I strongly recommend that you use the <strong>ESEM code generator for Mplus</strong> (<a href="https://www.surveyhost.co.za/esem/">https://www.surveyhost.co.za/esem/</a>). This is an extremely powerful tool that automatically generates ESEM, Bi-factor ESEM, ESEM within CFA, and Hierarchical ESEM code when you input the relevant variables from your study.</p>

<h4>Example 1: Basic ESEM Model</h4>

<pre>
TITLE: This is an example of an ESEM
DATA: FILE IS CFA for CES-D.dat;
VARIABLE: NAMES = age gender y1-y20;
MODEL:  F1-F3 BY y1-y20 (*1); 
! The asterisk in parentheses defines F1-F3 as ESEM factors
OUTPUT: STANDARDIZED MOD;
</pre>

<h4>Example 2: ESEM with Covariate</h4>

<pre>
TITLE: This is an example of an ESEM with covariate
DATA: FILE IS CFA for CES-D.dat;
VARIABLE: NAMES = age y1-y20 i1-i10;
ANALYSIS: ROTATION = geomin(oblique);            
ESTIMATOR = MLR;
MODEL:  F1-F3 BY y1-y20 (*1); F4 BY i1-i5; F1-F3 ON F4; 
OUTPUT: STANDARDIZED MOD;
</pre>

<h4>Example 3: Complex ESEM Model with Multiple Variables</h4>
<pre>
TITLE: This is an example of an ESEM
DATA: FILE IS CFA for CES-D.dat;
VARIABLE: NAMES = age gender y1-y20 i1-i10; 
USEVARIABLES = y1-y20 i1-i5;
ANALYSIS:       
ESTIMATOR = MLR;
MODEL: 
    F1 BY y1* y2 y3 y5 y6 y7 y9 y10 y11;
    F2 BY y4* y8 y12 y16;
    F3 BY y13* y14 y15 y17 y18 y19 y20;
    F4 BY i1* i2-i5;
    F1-F4@1;
    F1-F3 ON F4;
OUTPUT: STANDARDIZED MOD;
</pre>
<p>If you want to know more, I suggest you read the following articles and books:
Asparouhov,T., & Muthén, B. (2009). Exploratory structural equation modeling. Structural Equation Modeling: A Multidisciplinary Journal, 16(3), 397-438.Mai,Y., Zhang, Z., & Wen, Z. (2018). Comparing exploratory structural equation modeling and existing approaches for multiple regression with latent variables. Structural Equation Modeling: A Multidisciplinary Journal, 25(5), 737-749.Marsh,H. W., Guo, J., Dicke, T., Parker, P. D., & Craven, R. G. (2020). Confirmatory factor analysis (CFA), exploratory structural equation modeling(ESEM), and set-ESEM: optimal balance between goodness of fit and parsimony. Multivariate Behavioral Research, 55(1), 102-119.
