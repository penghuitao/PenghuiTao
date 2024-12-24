---
layout: page
title:  "Structural Equation Modeling"
subtitle: ""
date:   2022-11-11 15:01:21 +0530
categories: ["Structural Equation Modeling"]
---


<p>Structural Equation Modeling (SEM) is a method for establishing, estimating, and testing causal relationship models. These models include both observable manifest variables and potentially unobservable latent variables. SEM can replace multiple regression, path analysis, factor analysis, and covariance analysis methods, providing a clear analysis of the effects of individual indicators on the overall model and the relationships between individual indicators.</p>
<img src="{{ '/assets/img/20240123/SEM1.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>SEM consists of two parts: the measurement model and the structural model. In previous articles, I discussed the measurement model, particularly Confirmatory Factor Analysis (CFA). In this article, we will explore the complete SEM model. Common packages used for SEM include <code>psych</code>, <code>sem</code>, <code>OpenMx</code>, and <code>lavaan</code>. Since we previously discussed path analysis using the <code>lavaan</code> package, I will continue to use this package here.</p>

<h3><strong>1. Load the Data</strong></h3>
<pre><code>
library(readxl)
coping <- read_excel("jealousy.xls")
str(coping)
</code></pre>
<img src="{{ '/assets/img/20240123/SEM2.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h3><strong>2. Create the Model</strong></h3>
<p>Based on previous research, we hypothesize that self-esteem has a significant positive predictive effect on jealousy, and both positive and negative coping strategies mediate this relationship.</p>
<pre><code>
library(lavaan)
model <- '
jealousy =~ b1 + b2 + b3 + b4 + b5 + b6 + b7 + b8 + b9 + b10 + b11 + b12 + b13 + b14 + b15 + b16 + b17 + b18 + b19 + b20
self_esteem =~ z1 + z2 + z3 + z4 + z5 + z6 + z7 + z8 + z9 + z10
positive_coping =~ d1 + d2 + d3 + d4 + d5 + d6 + d7 + d8 + d9 + d10 + d11 + d12
negative_coping =~ d13 + d14 + d15 + d16 + d17 + d18 + d19 + d20
jealousy ~ a*self_esteem + b*positive_coping + c*negative_coping
positive_coping ~ d*self_esteem
negative_coping ~ e*self_esteem
ind1 := b*d  # Indirect effect through positive coping
ind2 := c*e  # Indirect effect through negative coping
'
</code></pre>

<h3><strong>3. Estimate and View the Model</strong></h3>
<pre><code>
fit <- sem(model, data = coping, se = "bootstrap", bootstrap = 200)  # Set bootstrap to 200
summary(fit, standardized = TRUE, fit = TRUE, rsquare = TRUE)  # View standardized results and model fit
</code></pre>
<img src="{{ '/assets/img/20240123/SEM3.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>The results show that the model's fit is not ideal. Specifically, the CFI = 0.691, TLI = 0.676.</p>
<img src="{{ '/assets/img/20240123/SEM4.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h3><strong>4. Adjust the Model</strong></h3>
<p>Given the poor fit of the model, we need to adjust it. In this case, we will keep the mediating effect of negative coping and remove positive coping from the SEM. In practice, model adjustments are more complex and may involve techniques like item parceling to improve the fit indices.</p>
<pre><code>
model1 <- '
jealousy =~ b1 + b2 + b3 + b4 + b5 + b6 + b7 + b8 + b9 + b10 + b11 + b12 + b13 + b14 + b15 + b16 + b17 + b18 + b19 + b20
self_esteem =~ z1 + z2 + z3 + z4 + z5 + z6 + z7 + z8 + z9 + z10
negative_coping =~ d13 + d14 + d15 + d16 + d17 + d18 + d19 + d20
jealousy ~ a*self_esteem + c*negative_coping
negative_coping ~ e*self_esteem
ind2 := c*e  # Indirect effect through negative coping
'
</code></pre>

<h3><strong>5. Estimate and View the Adjusted Model</strong></h3>
<pre><code>
fit1 <- sem(model1, data = coping, se = "bootstrap", bootstrap = 200)  # Set bootstrap to 200
summary(fit1, standardized = TRUE, fit = TRUE, rsquare = TRUE)  # View standardized results and model fit
</code></pre>
<img src="{{ '/assets/img/20240123/SEM5.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<img src="{{ '/assets/img/20240123/SEM6.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>We observe that the model's fit has worsened. Therefore, removing positive coping from the model seems to be problematic. Other adjustments, such as allowing error correlations, might be necessary to improve the fit indices. Since model tuning is time-consuming, I will not discuss it further here. You can conduct related research and refer to the references below:
<br>Bowen, N. K., & Guo, S. (2011). <i>Structural equation modeling</i>. Oxford University Press.
<br>Cheung, G. W., Cooper-Thomas, H. D., Lau, R. S., & Wang, L. C. (2023). Reporting reliability, convergent and discriminant validity with structural equation modeling: A review and best-practice recommendations. <i>Asia Pacific Journal of Management, 41</i>(2), 745–783. <a href="https://doi.org/10.1007/s10490-023-09871-y">https://doi.org/10.1007/s10490-023-09871-y</a>
<br>Hoyle, R. H. (Ed.). (1995). <i>Structural equation modeling: Concepts, issues, and applications</i>. Sage.</p>
