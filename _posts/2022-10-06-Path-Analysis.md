---
layout: page
title:  "Path Analysis"
subtitle: ""
date:   2022-10-06 15:01:21 +0530
categories: ["Structural Equation Modeling"]
---


<p>Path analysis is a direct extension of multiple regression. The aim is to estimate the magnitude and direction of the hypothesized causal relationships between variables using path diagrams.</p>

<p>Conducting path analysis in R generally involves four steps:</p>
<ol>
  <li>Load the data (as a correlation matrix or raw data).</li>
  <li>Specify the model.</li>
  <li>Estimate the model.</li>
  <li>View the results.</li>
</ol>

<p>In R, packages such as <code>sem</code>, <code>psych</code>, <code>OpenMx</code>, and <code>lavaan</code> can be used for path analysis and structural equation modeling. The most commonly used package is <code>lavaan</code>, which has a simple syntax, is easy to learn, supports non-normal continuous data, and can handle missing values. The name <code>lavaan</code> comes from "latent variable analysis," derived from the first two letters of each word: la-va-an—lavaan.</p>

<h3><strong>1. Load the Data</strong></h3>
<p>We continue using the jealousy study dataset as an example.</p>
<img src="{{ '/assets/img/20240123/PAA1.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h3><strong>2. Specify the Model</strong></h3>
<p>Based on previous research, we hypothesize that both positive and negative coping strategies mediate the relationship between self-esteem and jealousy. The model is as follows:</p>

<pre><code>
library(lavaan)
pathmodel1 <- "
positiveCP ~ SelfEsteem
negativeCP ~ SelfEsteem
jealou ~ positiveCP + negativeCP + SelfEsteem
"
</code></pre>
<img src="{{ '/assets/img/20240123/PAA2.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>Here, <code>pathmodel1</code> is the model being specified. <code>positiveCP ~ SelfEsteem</code> represents the effect of self-esteem on positive coping, <code>negativeCP ~ SelfEsteem</code> represents the effect of self-esteem on negative coping, and <code>jealou ~ positiveCP + negativeCP + SelfEsteem</code> represents the effect of self-esteem, positive coping, and negative coping on jealousy.</p>
<img src="{{ '/assets/img/20240123/PAA3.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h3><strong>3. Estimate the Model</strong></h3>
<p>After specifying the model, we need to estimate it. The syntax is:</p>

<pre><code>
pathfit <- sem(pathmodel1, mydata2)  # pathmodel is the specified model; mydata2 is the dataset
summary(pathfit, fit.measures = "TRUE")
</code></pre>

<p>First, check the model fit:</p>
<pre>
CFI = 0.911, SRMR = 0.036, TLI = 0.466, RMSEA = 0.100.
</pre>
<img src="{{ '/assets/img/20240123/PAA4.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>This suggests the model fit is not perfect. Next, examine the model’s path coefficients.</p>

<h3><strong>4. View the Results</strong></h3>
<p>Step 4 is actually performed simultaneously with Step 3.</p>
<img src="{{ '/assets/img/20240123/PAA5.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>Results indicate that self-esteem has a significant negative predictive effect on negative coping and no significant predictive effect on positive coping. Positive coping does not significantly predict jealousy, but both self-esteem and negative coping significantly predict jealousy.</p>

<p>We can also view all fit indices:</p>
<img src="{{ '/assets/img/20240123/PAA6.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<pre><code>
fitMeasures(pathfit, fit.measures = "all", baseline.model = NULL)
</code></pre>
<img src="{{ '/assets/img/20240123/PAA7.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>The model’s NFI = 0.903 and IFI = 0.918.</p>

<p>To get standardized solutions:</p>
<img src="{{ '/assets/img/20240123/PAA8.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<pre><code>
standardizedSolution(pathfit)
</code></pre>

<p>This includes the standardized path coefficients and confidence intervals.</p>

<h3><strong>5. Using Bootstrap for Confidence Intervals</strong></h3>
<p>Since published papers often require reporting confidence intervals for indirect effects, we demonstrate how to use the bootstrap method for path analysis.</p>

<h4><strong>5.1 Specify the Model</strong></h4>
<p>In <code>pathmodel2</code>, we introduce the path coefficients for each predictor.</p>
<img src="{{ '/assets/img/20240123/PAA9.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<pre><code>
pathmodel2 <- "
positiveCP ~ a * SelfEsteem
negativeCP ~ b * SelfEsteem
jealou ~ c * positiveCP + d * negativeCP + e * SelfEsteem
Indirect1 := a * c  # Indirect effect through positive coping
Indirect2 := b * d  # Indirect effect through negative coping
"
</code></pre>
<img src="{{ '/assets/img/20240123/PAA10.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h4><strong>5.2 Set the Bootstrap Algorithm</strong></h4>

<pre><code>
results1 <- sem(pathmodel2, data = mydata2, se = "bootstrap", bootstrap = 100)
</code></pre>

<h4><strong>5.3 Estimate and View the Model</strong></h4>
<img src="{{ '/assets/img/20240123/PAA11.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<pre><code>
summary(results1, standardized = TRUE, fit = TRUE, rsquare = TRUE)
</code></pre>

<p>Results show that self-esteem has a significant indirect effect on jealousy through both positive and negative coping strategies, with confidence intervals that do not include 0.</p>

<h3><strong>6. Model Comparison</strong></h3>

<h4><strong>6.1 Specify the Model</strong></h4>
<p>Given that the effect of self-esteem on positive coping is not significant, we remove positive coping from the simpler model:</p>

<pre><code>
pathmodel3 <- "
negativeCP ~ b * SelfEsteem
jealou ~ d * negativeCP + e * SelfEsteem
Indirect2 := b * d
Direct := e
Total := e + b * d
"
</code></pre>
<img src="{{ '/assets/img/20240123/PAA12.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h4><strong>6.2 Compare the Models</strong></h4>
<p>Use the <code>anova</code> function to compare nested models:</p>
<pre><code>
results2 <- sem(pathmodel3, data = mydata2, se = "bootstrap", bootstrap = 100)
anova(results1, results2)  # Compare the bootstrap models
</code></pre>

<p>The AIC for model 2 is lower than that for model 1, indicating a better fit for model 2. Additionally, the Chi-square difference of 5.92 (p = 0.01497) suggests a significant difference between the models.</p>

<h3><strong>7. Generate Model Diagrams</strong></h3>
<p>To generate model diagrams, we use the <code>semPlot</code> package:</p>

<pre><code>
library(semPlot)
semPaths(results1, "std", layout = "tree2")  # Tree diagram
</code></pre>
<img src="{{ '/assets/img/20240123/PAA13.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<pre><code>
semPaths(results1, "std", layout = "circle")  # Circular diagram
</code></pre>
<img src="{{ '/assets/img/20240123/PAA14.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>These diagrams represent the path model visually.</p>
