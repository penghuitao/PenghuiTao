---
layout: page
title:  "Introduction to Factor Analysis: EFA and CFA"
subtitle: ""
date:   2023-05-01 21:21:21 +0530
categories: ["Factor Analysis"]
---

<p>In this series, I will introduce factor analysis. To help you better understand the following articles, I will briefly introduce the basic knowledge of factor analysis. Similarly, if you have any questions, please feel free to contact me at any time.</p>
<h2><strong>What is Factor Analysis</strong></h2>
<h3><strong>Meaning</strong></h3>
<p>Factor Analysis is a statistical technique that reduces multiple observable variables (i.e., observed variables) that describe an entity into a few unobservable variables (i.e., latent variables, also known as latent factors).</p>
<ul>
  <li><strong>Observed variables</strong>: Variables that can be directly measured. For example, a question in a questionnaire or a dimension score that is directly summed up, without estimating error.</li>
  <li><strong>Latent variables</strong>: These are typically variables that cannot be directly measured, and must be estimated using external measurement indicators. Similar concepts include constructs, traits, and factors.</li>
</ul>

<h3><strong>Example</strong></h3>
<p>For instance, a student's academic ability (latent variable) can be described by their subject scores (observed variables), such as Chinese, Mathematics, and English scores.</p>

<p>Factor analysis explores the internal relationships between numerous variables to uncover the underlying structure of observed variables. It represents the data's fundamental structure using a few meaningful, independent factors. This is why factor analysis is often referred to as a "dimension reduction" technique — it uses a few more generalizable factors to reduce the original dimensions. These factors reflect the main information represented by the original observed variables and explain the interdependence between these variables.</p>

<h2><strong>Types of Factor Analysis</strong></h2>
<p>Factor analysis can be divided into two major types: Exploratory Factor Analysis (EFA) and Confirmatory Factor Analysis (CFA), depending on whether there is a theoretical foundation before analysis.</p>

<h3><strong>Exploratory Factor Analysis (EFA)</strong></h3>
<p>The core of Exploratory Factor Analysis (EFA) is to uncover the latent structure in the data. It uses methods like Principal Component Analysis (PCA) or Common Factor Analysis to break down observed variables into a few underlying factors. These factors represent the shared variance among the observed variables and can be interpreted as latent constructs. One of the advantages of EFA in research design is its exploratory nature; it allows researchers to discover potential structural relationships between variables without a prior hypothesis. However, EFA also has limitations, such as its inability to directly test theoretical models, and the choice of factor rotation method can influence the interpretation of results.</p>

<h3><strong>Confirmatory Factor Analysis (CFA)</strong></h3>
<p>In contrast to EFA, Confirmatory Factor Analysis (CFA) is a more rigorous statistical method that requires researchers to specify a theoretical model before analysis. CFA uses Structural Equation Modeling (SEM) to assess the goodness-of-fit between the observed data and the theoretical model, thereby testing theoretical hypotheses. The strength of CFA lies in its confirmatory nature, as it provides model fit indices to help researchers assess the quality of the model. However, CFA has higher data requirements, including larger sample sizes, and it is very sensitive to model specification.</p>

<h3>Comparison Between EFA and CFA</h3>
<p>EFA and CFA differ significantly in their methodology and are suited for different stages and purposes of research. EFA is used for theoretical exploration and preliminary studies, while CFA is used for theoretical validation and model comparison. The choice of the appropriate factor analysis method depends on the research purpose, data characteristics, and theoretical foundation. In some cases, researchers may use both EFA and CFA in sequence—first using EFA to explore the data structure, and then using CFA to validate the theoretical model. This article will demonstrate, through case studies, how to choose and combine EFA and CFA based on the specific research context.</p>

<p>If the psychological behavior variables are unclear and there is little knowledge about the factors underlying the observed data, EFA can help simplify the data and understand the specific factors included in the psychological behavior variable. If the researcher has already hypothesized the format and structure of the psychological behavior variable based on theory or prior research, CFA can be used to determine whether the observed data truly measure these factors and whether the existing theoretical hypotheses can be validated.</p>

<h2><strong>Theoretical Model Assumptions of Factor Analysis</strong></h2>
<p>Observed indicators (questions, items, survey items) consist of two parts:</p>
<ul>
  <li><strong>Common Factors / Public Factors</strong>: The shared portion of all items or entries.</li>
  <li><strong>Unique Factors</strong>: Factors unique to each item, where there is no correlation between the unique factors of different items.</li>
</ul>

<p>There is no correlation between unique factors, and no correlation between unique factors and common factors. The relationship between common factors may be correlated or uncorrelated.</p>

<h3><strong>Key Concepts</strong></h3>
<ul>
  <li><strong>Factor Loading</strong>: The correlation coefficient between the observed variable and the extracted common factor. The square of the factor loading can explain the proportion of variance in the observed variable that is explained by a particular factor.</li>
  <li><strong>Communality</strong>: The percentage of variance in an observed variable explained by common factors. It represents the degree to which the original information in the variable is retained after replacing the variable with the common factors.</li>
  <li><strong>Eigenvalue</strong>: The extent to which a common factor influences each observed variable, reflecting the importance of that common factor.</li>
</ul>

<h3><strong>Purpose</strong></h3>
<ul>
  <li>To represent a large number of variables with fewer factors.</li>
  <li>To explore and construct the structure of psychological constructs (EFA).</li>
  <li>To test the structural validity of psychological tests (CFA).</li>
</ul>

<p>If you want to know more, I suggest you read the following articles and books:
<br>Kline, P. (2014). <i>An easy guide to factor analysis</i>. Routledge.
<br>Pett, M. A., Lackey, N. R., & Sullivan, J. J. (2003). <i>Making sense of factor analysis: The use of factor analysis for instrument development in health care research</i>. sage.
<br>Shrestha, N. (2021). Factor analysis as a tool for survey analysis. <i>American Journal of Applied Mathematics and Statistics, 9</i>(1), 4–11. https://doi.org/10.12691/ajams-9-1-2
<br>Rummel, R. J. (1988). <i>Applied factor analysis</i>. Northwestern University Press.
