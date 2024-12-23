---
layout: page
title:  "Moderated Mediation Model"
subtitle: ""
date:   2024-04-07 22:00:21 +0530
categories: ["Mediation Analysis"]
---


<p>In previous articles, I introduced simple mediation models, parallel mediation models, and chain mediation models. Today, I will introduce a slightly more complex mediation model: the moderated mediation model.</p>

<p>A moderated mediation model is a common model that simultaneously includes both a mediator variable and a moderator variable. This model suggests that the independent variable influences the dependent variable through the mediator variable, while the mediation process is moderated by the moderator variable (Baron & Kenny, 1986). In this article, I will use a specific research example to demonstrate how to implement this model in R.</p>
<img src="{{ '/assets/img/20240123/MMMA.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>Research shows that there is a similarity between adolescents' and mothers' personalities, which seems to validate the saying "like mother, like son." However, in real life, we also find that there are differences between adolescents' and mothers' personalities. Our study focuses on the influence of mothers' neuroticism on adolescents' neuroticism, as well as the roles of parenting style (mediator) and peer relationships (moderator). Our hypothesized model is as follows:</p>

<ul>
  <li><strong>X</strong> = Mother's neuroticism (MN)</li>
  <li><strong>M</strong> = Warm parenting style (WP)</li>
  <li><strong>W</strong> = Peer relationships (PR)</li>
  <li><strong>Y</strong> = Adolescent neuroticism (CN)</li>
</ul>

<h3><strong>1. Load the Data</strong></h3>
<pre><code>
data <- read.csv(file.choose(), header = TRUE)  # Select the dataset from your path
head(data)
</code></pre>
<img src="{{ '/assets/img/20240123/MMMB.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h3><strong>2. Calculate Correlations</strong></h3>
<pre><code>
corr.test(data[, 3:6])  # Since the variables are in columns 3 to 6
</code></pre>
<img src="{{ '/assets/img/20240123/MMMC.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>The results show that all correlation coefficients are significant. Notably, mother's neuroticism is positively correlated with adolescent neuroticism, and negatively correlated with warm parenting and peer relationships. Adolescents' neuroticism is negatively correlated with warm parenting and peer relationships. Warm parenting is positively correlated with peer relationships.</p>

<h3><strong>3. Test the Moderated Mediation Model</strong></h3>

<h4>3.1 Center the Variables</h4>
<pre><code>
centerPR <- scale(PR, center = TRUE, scale = FALSE)  # Center peer relationships
centerWP <- scale(WP, center = TRUE, scale = FALSE)  # Center warm parenting style
centermw <- centerPR * centerPR  # Calculate the interaction term
</code></pre>

<h4>3.2 Define the Model</h4>
<pre><code>
pathmodel5 <- "
WP ~ a*MN
CN ~ b*MN + c*WP + d*PR + e*centermw
ind := a*(c + e)  # Mediation effect when peer relationship = 1
ind1 := a*(c - e)  # Mediation effect when peer relationship = -1
"
</code></pre>

<h4>3.3 Estimate and View Results</h4>
<pre><code>
results2 <- sem(pathmodel5, data = data, se = "bootstrap", bootstrap = 100)  # Using bootstrap method
summary(results2, standardized = TRUE, fit = TRUE, rsquare = TRUE)
</code></pre>
<img src="{{ '/assets/img/20240123/MMMD.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<h4>3.4 Calculate the Standardized Solution</h4>
<pre><code>
standardizedSolution(results2)
</code></pre>
<img src="{{ '/assets/img/20240123/MMME.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>The results show that at different levels of peer relationships, the indirect effect of mother's neuroticism on adolescent neuroticism through warm parenting is significant. However, as peer relationships decrease, the effect of mother's neuroticism on adolescent neuroticism also diminishes. This may be due to the fact that peer relationships, as a stable external factor, play a "protective role" in the "genetic" transmission of personality traits between family members.</p>

<h4>3.5 Plot the Model</h4>
<pre><code>
semPaths(results2, 'std', layout = 'circle')
</code></pre>
<img src="{{ '/assets/img/20240123/MMMF.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">

<p>If you want to learn more, I recommend reading the following articles:
<br>Alfes, K., Shantz, A. D., Truss, C., & Soane, E. C. (2012). The link between perceived human resource management practices, engagement and employee behaviour: a moderated mediation model. <i>The International Journal of Human Resource Management, 24</i>(2), 330–351. <a href="https://doi.org/10.1080/09585192.2012.679950">https://doi.org/10.1080/09585192.2012.679950</a>
<br>Hayes, A. F. (2015). An index and test of linear moderated mediation. <i>Multivariate Behavioral Research, 50</i>(1), 1–22. <a href="https://doi.org/10.1080/00273171.2014.962683">https://doi.org/10.1080/00273171.2014.962683</a>
<br>Wen, Z., & Ye, B. (2014). Different methods for testing moderated mediation models: competitors or backups? <i>Acta Psychologica Sinica, 46</i>(5), 714. <a href="https://doi.org/10.3724/sp.j.1041.2014.00714">https://doi.org/10.3724/sp.j.1041.2014.007143</a>
<br>If you have any questions, feel free to email me.</p>
