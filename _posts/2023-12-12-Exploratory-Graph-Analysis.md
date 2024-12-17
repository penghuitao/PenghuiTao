---
layout: page
title:  "Visualizing Connections: A Guide to Exploratory Graph Analysis"
subtitle: ""
date:   2023-12-12 15:01:21 +0530
categories: ["Network Analysis"]
---


<h2><strong>Why Do We Need Exploratory Graph Analysis?</strong></h2>
<p>Dimensional estimation has always been an important task in psychometrics, with close ties to the development of intelligence tests and the dimensional division of intelligence theories. Existing dimensional estimation methods, such as the Kaiser-Guttman criterion (eigenvalues greater than 1), Horn's parallel analysis, and using BIC or EBIC to compare the fit of structural models with different numbers of factors (i.e., factor analysis), perform quite well when the sample size is large (typically ≥500), factor correlations are low or moderate, and factor loadings are high or moderate. However, these methods tend to overestimate the number of factors when factor correlations are high, sample sizes are small, and the number of items per factor is limited.</p>

<h2><strong>What is Exploratory Graph Analysis (EGA)?</strong></h2>
<p>EGA, short for Exploratory Graph Analysis, is a newly emerging method for estimating dimensions in psychometric data using network estimation techniques and community detection algorithms. The development of EGA stemmed from academic discussions comparing the significance of factor loadings in factor analysis and centrality in network analysis. A common phenomenon in factor analysis (dimensional estimation) is the high correlation between factors, which often aligns with practical experience. Researchers typically hope for strong correlations within factors and weak (or even no) correlations between factors, as this would greatly improve model fit.</p>

<p>The Least Absolute Shrinkage and Selection Operator (LASSO) is a widely used estimation method in statistical modeling to prevent overfitting. By using LASSO, small correlations between factors are penalized and estimated as zero. In other words, partial correlations between items within different factors may reflect spurious correlations (e.g., A and C are correlated because they are both related to B). LASSO shrinks small correlation coefficients to zero to indicate conditional independence, thereby enhancing the model's interpretability. The most commonly used model in EGA is the GGM (Graphical Gaussian Model), which is also the most common LASSO graphical model in network analysis. On this basis, EGA uses clustering algorithms to detect and estimate the number of dimensions.</p>

<h2><strong>What Are the Advantages of Exploratory Graph Analysis?</strong></h2>
<ul>
  <li><strong>Visualization ability</strong>: EGA provides a visual network graph with color-coded entries, making it easy to identify the dimensional grouping of items.</li>
  <li><strong>Automatic estimation</strong>: No need to worry about which rotation method to use.</li>
  <li><strong>Stability testing</strong>: Stability can be assessed through bootstrap methods.</li>
</ul>
<p>As a result, Cosemans et al. (2022) recommend EGA as the preferred method for estimating dimensions in continuous data.</p>

<h2><strong>How to Implement It in R?</strong></h2>
  <li>First, you need to install the <strong>EGAnet</strong> package in R. You can search for "EGAnet" directly in R:</li>
  <img src="{{ '/assets/img/penghui.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <li>Then, read through the documentation provided by the authors. You will find that using EGA is quite simple. Here is an image showing the most basic form of EGA:</li>
  <img src="{{ '/assets/img/penghui.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <li>However, I recommend using bootstrap EGA directly, as it offers more advantages:</li>
  <img src="{{ '/assets/img/penghui.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <li>Finally, if you follow the steps correctly, you will obtain text information and a network graph with dimensional information:</li>
  <img src="{{ '/assets/img/penghui.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <img src="{{ '/assets/img/penghui.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
