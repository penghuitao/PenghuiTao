---
layout: page
title:  "CLPM and RI-CLPM R Implementation"
subtitle: ""
date:   2024-09-01 01:45:01 +0530
categories: ["Longitudinal Analysis"]
---

r
# Install and load the lavaan package
# install.packages("lavaan")
library(lavaan)

# Load the dataset
dat <- read.table("RICLPM.dat", col.names = c("x1", "x2", "x3", "x4", "x5", "y1", "y2", "y3", "y4", "y5"))

# Convert the wide data format to long format
library(tidyverse)
datLong <- dat %>%
  mutate(pid = 1:n()) %>%
  pivot_longer(x1:y5, names_to = "vars", values_to = "value") %>%
  extract(col = vars, into = c('var', 'wave'), regex = '(\\w)(\\d)')

# Plot variable distribution across waves
datLong %>%
  ggplot(aes(x = value)) + 
  geom_density(alpha = 1/3, size = 1, color = "blue") +
  facet_grid(wave ~ var, scales = 'free') +
  theme(panel.background = element_rect(fill = "white"))

# Plot trend of variable changes over waves
ggplot(data = datLong, aes(x = wave, y = value, color = var, group = var)) +
  geom_point(shape = 1) +
  stat_summary(fun = mean, geom = "line", lwd = 1.5) +
  stat_summary(fun = mean, geom = "point", size = 3, shape = 19) +
  theme(panel.background = element_rect(fill = "white"))

# Define CLPM model
CLPM <- '
  # Estimate the lagged effects between the observed variables
  x2 + y2 ~ x1 + y1
  x3 + y3 ~ x2 + y2
  x4 + y4 ~ x3 + y3
  x5 + y5 ~ x4 + y4
  
  # Estimate the covariance between the observed variables at the first wave
  x1 ~~ y1
  
  # Estimate the covariances between the residuals of the observed variables
  x2 ~~ y2
  x3 ~~ y3
  x4 ~~ y4
  x5 ~~ y5
  
  # Estimate the (residual) variance of the observed variables
  x1 ~~ x1
  y1 ~~ y1
  x2 ~~ x2
  y2 ~~ y2
  x3 ~~ x3
  y3 ~~ y3
  x4 ~~ x4
  y4 ~~ y4
  x5 ~~ x5
  y5 ~~ y5
'

# Estimate CLPM model
CLPM.fit <- sem(CLPM, data = dat, missing = "fiml", mimic = "mplus", se = "boot", bootstrap = 1000, meanstructure = TRUE, int.ov.free = TRUE)

# Report CLPM results
summary(CLPM.fit, standardized = TRUE)
parameterEstimates(CLPM.fit)
fitMeasures(CLPM.fit)

# Define the RI-CLPM model
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
  
  # Estimate the lagged effects between the within-person centered variables
  wx2 + wy2 ~ wx1 + wy1
  wx3 + wy3 ~ wx2 + wy2
  wx4 + wy4 ~ wx3 + wy3
  wx5 + wy5 ~ wx4 + wy4
  
  # Estimate the covariance between the within-person centered variables at the first wave
  wx1 ~~ wy1
  
  # Estimate the covariances between the residuals of the within-person centered variables (the innovations)
  wx2 ~~ wy2
  wx3 ~~ wy3
  wx4 ~~ wy4
  wx5 ~~ wy5
  
  # Estimate the variance and covariance of the random intercepts
  RIx ~~ RIx
  RIy ~~ RIy
  RIx ~~ RIy
  
  # Estimate the (residual) variance of the within-person centered variables
  wx1 ~~ wx1
  wy1 ~~ wy1
  wx2 ~~ wx2
  wy2 ~~ wy2
  wx3 ~~ wx3
  wy3 ~~ wy3
  wx4 ~~ wx4
  wy4 ~~ wy4
  wx5 ~~ wx5
  wy5 ~~ wy5
'

# Estimate the RI-CLPM model
RICLPM.fit <- sem(RICLPM, data = dat, missing = "fiml", mimic = "mplus", se = "boot", bootstrap = 1000, meanstructure = TRUE, int.ov.free = TRUE)

# Report RI-CLPM results
summary(RICLPM.fit, standardized = TRUE)
parameterEstimates(RICLPM.fit)
fitMeasures(RICLPM.fit)

# Compare the fit of CLPM and RI-CLPM models
library(semTools)
compareFit(CLPM.fit, RICLPM.fit)
、、、
