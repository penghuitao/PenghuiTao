---
layout: page
title:  "Moderated Mediation Model"
subtitle: ""
date:   2024-04-07 22:00:21 +0530
categories: ["Mediation Analysis"]
---


<p>In previous articles, I introduced simple mediation models, parallel mediation models, and chain mediation models. Today, I will introduce a slightly more complex mediation model: the moderated mediation model.</p>

<p>A moderated mediation model is a common model that simultaneously includes both a mediator variable and a moderator variable. This model suggests that the independent variable influences the dependent variable through the mediator variable, while the mediation process is moderated by the moderator variable (Baron & Kenny, 1986). In this article, I will use a specific research example to demonstrate how to implement this model in R.</p>

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

<h3><strong>2. Calculate Correlations</strong></h3>
<pre><code>
corr.test(data[, 3:6])  # Since the variables are in columns 3 to 6
</code></pre>

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

<h4>3.4 Calculate the Standardized Solution</h4>
<pre><code>
standardizedSolution(results2)
</code></pre>

<p>The results show that at different levels of peer relationships, the indirect effect of mother's neuroticism on adolescent neuroticism through warm parenting is significant. However, as peer relationships decrease, the effect of mother's neuroticism on adolescent neuroticism also diminishes. This may be due to the fact that peer relationships, as a stable external factor, play a "protective role" in the "genetic" transmission of personality traits between family members.</p>

<h4>3.5 Plot the Model</h4>
<pre><code>
semPaths(results2, 'std', layout = 'circle')
</code></pre>


<p>If you want to learn more, I recommend reading the following articles, all of which use the chain mediation model:
<br>Chen, K., Huang, L., & Ye, Y. (2022). Research on the relationship between wellness tourism experiencescape and revisit intention: a chain mediation model. <i>International Journal of Contemporary Hospitality Management, 35</i>(3), 893–918. <a href="https://doi.org/10.1108/ijchm-01-2022-0050">https://doi.org/10.1108/ijchm-01-2022-0050</a>
<br>Gori, A., Topino, E., & Di Fabio, A. (2020). The protective role of life satisfaction, coping strategies and defense mechanisms on perceived stress due to COVID-19 emergency: A chained mediation model. <i>PLoS ONE, 15</i>(11), e0242402. <a href="https://doi.org/10.1371/journal.pone.0242402">https://doi.org/10.1371/journal.pone.02424020</a>
<br>Wang, C., Chudzicka-Czupała, A., Tee, M. L., Núñez, M. I. L., Tripp, C., Fardin, M. A., Habib, H. A., Tran, B. X., Adamus, K., Anlacan, J., García, M. E. A., Grabowski, D., Hussain, S., Hoang, M. T., Hetnał, M., Le, X. T., Ma, W., Pham, H. Q., Reyes, P. W. C., . . . Sears, S. F. (2021). A chain mediation model on COVID-19 symptoms and mental health outcomes in Americans, Asians and Europeans. <i>Scientific Reports, 11</i>(1). <a href="https://doi.org/10.1038/s41598-021-85943-7">https://doi.org/10.1038/s41598-021-85943-7</a>
<br>If you have any questions, feel free to email me.</p>
