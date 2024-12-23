---
layout: page
title:  "CLPM and RI-CLPM R Implementation"
subtitle: ""
date:   2024-09-01 01:45:01 +0530
categories: ["Longitudinal Analysis"]
---


<h3>CLPM and RI-CLPM R Implementation</h3>

<h4>Traditional CLPM Lavaan Implementation</h4>
<p><strong>Loading Packages</strong></p>
<pre>
library(tidyverse)
library(lavaan)
</pre>

<p><strong>Reading Data</strong></p>
<pre>
dat <- read_table("RICLPM.dat", col_names = F) %>%   
rename(y1 = X6, y2 = X7, y3 = X8, y4 = X9, y5 = X10, x1 = X1, x2 = X2, x3 = X3, x4 = X4, x5 = X5)
</pre>

<p><strong>Transforming Wide to Long Format</strong></p>
<pre>
datLong <- dat %>%  mutate(pid = 1:n()) %>%  pivot_longer(x1:y5, names_to = "vars", values_to = "value") %>%  extract(col = vars, into = c('var', 'wave'), regex = '(\\w)(\\d)')
</pre>

<p><strong>Distribution of Variables Across Waves</strong></p>
<pre>
datLong %>%  ggplot(aes(x = value)) +  geom_density(alpha = 1/3, size = 1, color = "blue") + 
facet_grid(wave ~ var, scales = 'free') + theme(panel.background = element_rect(fill = "white"))
</pre>

<p><strong>Trend of Variable Changes</strong></p>
<pre>
ggplot(data = datLong, aes(x = wave, y = value, color = var, group = var)) + 
geom_point(shape = 1) + stat_summary(fun = mean, geom = "line", lwd = 1.5) + 
stat_summary(fun = mean, geom = "point", size=3, shape = 19) + 
theme(panel.background = element_rect(fill = "white"))
</pre>

<p><strong>Setting Up the Model</strong></p>
<pre>
CLPM <- '  
# Estimate the lagged effects between the observed variables.  
x2 + y2 ~ x1 + y1  
x3 + y3 ~ x2 + y2  
x4 + y4 ~ x3 + y3  
x5 + y5 ~ x4 + y4  

# Estimate the covariance between the observed variables at the first wave.   
x1 ~~ y1 # Covariance  

# Estimate the covariances between the residuals of the observed variables.  
x2 ~~ y2  
x3 ~~ y3  
x4 ~~ y4  
x5 ~~ y5  

# Estimate the (residual) variance of the observed variables.  
x1 ~~ x1 # Variances  
y1 ~~ y1  
x2 ~~ x2 # Residual variances  
y2 ~~ y2  
x3 ~~ x3  
y3 ~~ y3  
x4 ~~ x4  
y4 ~~ y4  
x5 ~~ x5  
y5 ~~ y5'
</pre>

<p><strong>Estimating the Model</strong></p>
<pre>
CLPM.fit <- sem(CLPM, data = dat, missing="fiml", mimic = "mplus", se = "boot", bootstrap = 1000, meanstructure = T, int.ov.free = T)
</pre>

<p><strong>Reporting Results</strong></p>
<pre>
summary(CLPM.fit, standardized = T)
parameterEstimates(CLPM.fit)
fitMeasures(CLPM.fit)
</pre>

<p>From the results, the model fit is within an acceptable range, and the x and y variables have a reciprocal causal relationship (however, it should be noted that cross-lagged correlations between the two variables not only reflect causal relationships but also include the temporal stability of each variable as well as the cross-correlation between the two variables at the first wave. If the stability of y is stronger than that of x, even if the coefficient for x1 → y2 is smaller than that of y1 → x2, the cross-lagged correlation between x1 and y2 could still be larger than between x2 and y1).</p>

<h4>Random Intercept Cross-Lagged Panel Model (RI-CLPM) Lavaan Implementation</h4>
<p>The RI-CLPM, compared to the traditional CLPM, requires the specification of four components:</p>
<ul>
  <li>The between-person component, i.e., the random intercepts. The factor loadings are fixed to 1 in the following part: <code>RIx =~ 1*x1 1*x2 ...</code></li>
  <li>The within-person component, which reflects individual fluctuations, can be set as <code>wx1 =~ 1*x1</code> and <code>wx2 =~ 1*x2</code>.</li>
  <li>The lagged regression coefficients: <code>wx2 ~ wx1 wy1;</code>, <code>wx3 ~ wx2 wy2;</code>.</li>
  <li>The covariances between the within- and between-person components. For example, <code>wx1 ~~ wy1;</code> indicates the covariance between the within-person components and the residuals, while <code>RIx ~~ RIy</code> indicates the covariance between the random intercepts.</li>
</ul>

<p><strong>Setting Up the RI-CLPM Model</strong></p>
<pre>
RICLPM <- '  
  # Create between components (random intercepts)  
  RIx =~ 1*x1 + 1*x2 + 1*x3 + 1*x4 + 1*x5  
  RIy =~ 1*y1 + 1*y2 + 1*y3 + 1*y4 + 1*y5  

  # Create within-person centered variables  
  wx1 =~ 1*x1  
  wx2 =~ 1*x2  
  wx3 =~ 1*x3  
  wx4 =~ 1*x4  
  wx5 =~ 1*x5  
  wy1 =~ 1*y1  
  wy2 =~ 1*y2  
  wy3 =~ 1*y3  
  wy4 =~ 1*y4  
  wy5 =~ 1*y5  

  # Estimate the lagged effects between the within-person centered variables.  
  wx2 + wy2 ~ wx1 + wy1  
  wx3 + wy3 ~ wx2 + wy2  
  wx4 + wy4 ~ wx3 + wy3  
  wx5 + wy5 ~ wx4 + wy4  

  # Estimate the covariance between the within-person centered variables at the first wave.   
  wx1 ~~ wy1 # Covariance  

  # Estimate the covariances between the residuals of the within-person centered variables (the innovations).  
  wx2 ~~ wy2  
  wx3 ~~ wy3  
  wx4 ~~ wy4  
  wx5 ~~ wy5  

  # Estimate the variance and covariance of the random intercepts.   
  RIx ~~ RIx  
  RIy ~~ RIy  
  RIx ~~ RIy  

  # Estimate the (residual) variance of the within-person centered variables.  
  wx1 ~~ wx1 # Variances  
  wy1 ~~ wy1  
  wx2 ~~ wx2 # Residual variances  
  wy2 ~~ wy2  
  wx3 ~~ wx3  
  wy3 ~~ wy3  
  wx4 ~~ wx4  
  wy4 ~~ wy4  
  wx5 ~~ wx5  
  wy5 ~~ wy5'
</pre>

<p><strong>Estimating the RI-CLPM Model</strong></p>
<pre>
RICLPM.fit <- lavaan(RICLPM, data = dat, missing="fiml", mimic = "mplus", se = "boot", bootstrap = 1000, meanstructure = T, int.ov.free = T)
</pre>

<p><strong>Reporting Results</strong></p>
<pre>
summary(RICLPM.fit, standardized = T)
parameterEstimates(RICLPM.fit)
fitMeasures(RICLPM.fit)
</pre>

<p>The results from the RI-CLPM show that the lagged coefficients for x → y are mostly significant, while the lagged coefficients for y → x are mostly not significant.</p>

<p><strong>Comparing the Fit of the Two Models</strong></p>
<pre>
semTools::compareFit(CLPM.fit, RICLPM.fit)
</pre>

<p>The results indicate that the RI-CLPM fits significantly better than the CLPM.</p>
