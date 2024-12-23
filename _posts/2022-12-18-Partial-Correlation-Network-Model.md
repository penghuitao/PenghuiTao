---
layout: page
title:  "Partial Correlation Network Model"
subtitle: ""
date:   2022-12-18 15:01:21 +0530
categories: ["Network Analysis"]
---


<p>Unlike latent variable models (which assume observed variables are influenced by latent variables), network analysis models represent individual traits (observed variables) in the form of a network, consisting of nodes and edges. This model can be used to illustrate the specific relationships between observed variables and provide a deeper understanding of the relationship between two traits.</p>

<p>Currently, this model is widely used in clinical psychology. For example, Fritz et al. (2018) compared network models of psychological resilience factors in adolescents with and without childhood adversity. Fried et al. (2015) conducted network research on grief and depressive symptoms, finding that loneliness was the most important factor linking the two.</p>

<p>Additionally, the most commonly used network analysis model today is the partial correlation network model.</p>
<p>For implementing network analysis models in R, one of the most commonly used packages is <code>qgraph</code>, and another useful package is <code>mgm</code>, which can be used to compare differences in network models between multiple groups of subjects. Below, I will demonstrate how to implement a partial correlation network model in R.</p>

<h2><strong>Code for Partial Correlation Network Model</strong></h2>
<pre>
# Load Required Packages
library(tidyverse)
library(qgraph)
library(readxl)

# Read and Transform Data
da <- read_xlsx("CES-D.xlsx", sheet = 1)

# Renaming Item Variables
names(da)[4:23] <- c("D1","S1","D2","P1","S2","D3","S3","P2","D4", "D5", 
                     "S4","P3","S5","D6","I1","P4","D7","D8","I2","S6")

# Reverse Code Positive Affect Items
da_f <- da %>%   
  mutate(P1 = recode(P1,'0'=3, '1' = 2, '2' = 1, '3' = 0),         
         P2 = recode(P2,'0'=3, '1' = 2, '2' = 1, '3' = 0),         
         P3 = recode(P3,'0'=3, '1' = 2, '2' = 1, '3' = 0),         
         P4 = recode(P4,'0'=3, '1' = 2, '2' = 1, '3' = 0)) %>%   
  select(D1,D2,D3,D4,D5,D6,D7,D8,S1,S2,S3,S4,S5,S6,P1,P2,P3,P4,I1,I2) %>%   
  drop_na()

# Calculate Partial Correlations and Plot Network
cor_da <- cor(da_f)
qgraph(cor_da, layout="spring",       
       groups = list("Depressed_Affect" = 1:8,                       
                     "Somatic_Complaints" = 9:14,                     
                     "Positive_Affect" = 15:18,                     
                     "Interpersonal_Problems" = 19:20),       
       graph = "pcor", vsize = 4, label.cex = 2)
</pre>

<p>The above plot shows the connections between all nodes in the network, which can become too complex and hard to interpret. Therefore, researchers often introduce a penalization factor in partial correlation network models, such as the GLASSO algorithm.</p>

<pre>
# Apply GLASSO Algorithm
qgraph(cor_da, layout="spring",       
       groups = list("Depressed_Affect" = 1:8,                       
                     "Somatic_Complaints" = 9:14,                     
                     "Positive_Affect" = 15:18,                     
                     "Interpersonal_Problems" = 19:20),       
       graph = "glasso", vsize = 4, label.cex = 2, sampleSize = nrow(da_f))
</pre>

<p>Additionally, we can present item names in the legend for a more intuitive representation of the relationships between items.</p>

<pre>
# Present Item Names in the Legend
Names <- scan("D:/future_plan/new_method/social network for variable relation/ces.txt", what = "character", sep = "\n")

qgraph(cor_da, layout="spring",                 
       groups = list("Depressed_Affect" = 1:8,                                
                     "Somatic_Complaints" = 9:14,                               
                     "Positive_Affect" = 15:18,                               
                     "Interpersonal_Problems" = 19:20),                  
       graph = "glasso", vsize = 4, label.cex = 2,                 
       sampleSize = nrow(da_f), nodeNames = Names, legend.cex = 0.2)
</pre>


<p>If you want to know more, I suggest you read the following articles and books:
<br>Epskamp, S., & Fried, E. I. (2018). A tutorial on regularized partial correlation networks. <i>Psychological Methods, 23</i>(4), 617–634. <a href="https://doi.org/10.1037/met0000167">https://doi.org/10.1037/met0000167</a>
<br>Fransson, P., & Marrelec, G. (2008). The precuneus/posterior cingulate cortex plays a pivotal role in the default mode network: Evidence from a partial correlation network analysis. <i>NeuroImage, 42</i>(3), 1178–1184. <a href="https://doi.org/10.1016/j.neuroimage.2008.05.059">https://doi.org/10.1016/j.neuroimage.2008.05.059</a>
<br>Williams, D. R., & Rast, P. (2019). Back to the basics: Rethinking partial correlation network methodology. <i>British Journal of Mathematical and Statistical Psychology, 73</i>(2), 187–212. <a href="https://doi.org/10.1111/bmsp.12173">https://doi.org/10.1111/bmsp.12173</a>

<p>If you have any questions, please feel free to contact me!</p>
