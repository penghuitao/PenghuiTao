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
data = read.table("ex9.36.dat", col.names = c("x1", "x2", "x3", "x4", "y1", "y2", "y3", "y4", 'subject'))

# Define the model
model = '  
  # Define regression paths (cross-lagged and autoregressive)
  x2 + y2 ~ x1 + y1
  x3 + y3 ~ x2 + y2
  x4 + y4 ~ x3 + y3
  # Define correlations between variables
  x1 ~~ y1
  x2 ~~ y2
  x3 ~~ y3
  x4 ~~ y4'

# Estimate the model
fit = sem(model, data = data)

# Check the model fit statistics
summary(fit, fit.measures = TRUE)

# Draw the path diagram (many methods available)
# The following method produces a diagram closer to Mplus output
library(tidySEM)
graph_sem(fit, layout = get_layout("x1", "x2", "x3", "x4", "y1", "y2", "y3", "y4", rows = 2))
