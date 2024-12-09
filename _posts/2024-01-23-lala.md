---
layout: page
title:  "Introduction to Temporal Network Analysis"
subtitle: "A truly foolproof tutorial"
date:   2024-01-23 21:21:21 +0530
categories: ["longitudinal analysis"]
---

<h2><strong>Preface</strong></h2>
<p>Network analysis has become increasingly common in the field of psychology, especially temporal network analysis for intensive longitudinal data, which has gained a lot of attention. In this article, I will provide a brief introduction to what temporal network analysis is and how to implement it in R. These codes may not be the most concise or optimal, but I can assure you they work properly (assuming you use them correctly in R). If you encounter any issues, please email me, and I will do my best to help you.</p>

<h2><strong>What is Temporal Network Analysis?</strong></h2>
<p>In the field of psychological research, network analysis methods were first applied to the study of psychological disorders (see Borsboom, 2017). However, so far, most psychological research involving network analysis has used cross-sectional networks. These studies primarily focus on the interactions between variables at a single point in time. However, most psychological variables evolve over time. Therefore, examining the relationships between variables longitudinally is an important direction for the development of network methods (Blanchard & Heeren, 2022; Bringmann et al., 2022). As is well known, longitudinal data in psychological research can be divided into two types: developmental process data and stable process data. These two types of data are obtained through different research designs and measurement methods, and their main differences can be seen in the table below. For panel data with 2-3 waves of tracking, cross-lagged network analysis is commonly used. For panel data with more than 5 waves of tracking and intensive longitudinal data, temporal network analysis is commonly employed.</p>

<p>Temporal network analysis methods are built upon the Vector Autoregression (VAR) model. Simply put, the "vector" in the VAR model refers to multiple features and is an extension of the univariate autoregressive model. The VAR model consists of a set of regression equations in which all variables are treated as endogenous. Therefore, they can serve both as independent and dependent variables. This allows VAR analysis to be conducted without prior assumptions about the direction of associations between variables. Through this method, we can uncover the cross-temporal dynamic relationships between multiple variables. Additionally, the residuals of the variables can further be used to estimate the correlation between variables at the same time point.</p>
<p>Based on the results of the VAR model, temporal network analysis can yield three types of network outcomes:</p>
<ol>
  <li><strong>Temporal network</strong>. In a temporal network, edges represent the predictive ability of a variable at a previous time point on the node at the next time point, after controlling for all other variables at the previous measurement. For example, as shown in the diagram, we can observe that sadness, intimacy with others, experiential avoidance, self-esteem, and paranoid beliefs all have autoregressive effects. Intimacy with others negatively predicts paranoia at the next time point, sadness positively predicts experiential avoidance, and experiential avoidance, in turn, positively predicts self-esteem at the next time point.</li>
<img src="{{ '/assets/img/Temporal_network.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <li><strong>Contemporaneous network</strong>. In a contemporaneous network, edges represent the associations between variables within the same time frame, after controlling for temporal associations. Several relationships stand out clearly, such as the positive correlation between paranoid beliefs and sadness, indicating that higher paranoid beliefs are associated with higher levels of sadness at the same time. Similarly, sadness is negatively correlated with both self-esteem and intimacy with others, meaning that the higher the sadness, the lower the self-esteem and intimacy with others. Additionally, self-esteem and intimacy with others are positively correlated. Finally, experiential avoidance shows no significant correlation with any other variable.</li>
<img src="{{ '/assets/img/Contemporaneous_network.png' | prepend: site.baseurl }}" id="about-img" style="width: 30%; max-width: 800px;">
  <li><strong>Between-subject network</strong>. In the between-subject network model, edges represent the correlations between the average levels of variables within individuals, after accounting for the remaining variables in the network. It can be observed that the average level of intimacy with others is negatively correlated with the average level of paranoid beliefs and positively correlated with the average level of self-esteem. The average level of sadness is negatively correlated with the average level of self-esteem and positively correlated with the average level of experiential avoidance.</li>
</ol>

<h2><strong>Implementation in R</strong></h2>
<p>In R, we use the mlVAR package to implement temporal network analysis. Here, we use data from the study <em>A Temporal Network Approach to Paranoia: A Pilot Study</em> (Contreras et al., 2020) as an example.</p>

<p>The study used an experience sampling design with temporal variation, where participants received a total of 10 questionnaire assessments each day from 9 AM to 10 PM over the course of 7 days. The longitudinal data is shown in the figure, where the <strong>subject</strong> variable represents the participant ID, the <strong>day</strong> variable represents the day number, and the <strong>beep</strong> variable represents the number of the questionnaire assessment.</p>

<p>To estimate the network, the mlVAR() function is used. The <strong>data</strong> argument refers to the longitudinal data shown above, <strong>vars</strong> includes the variables to be analyzed, <strong>idvar</strong> represents the participant ID variable, <strong>lag</strong> is the time lag, <strong>dayvar</strong> is the day variable, and <strong>beepvar</strong> indicates the number of the assessment. The <strong>estimator</strong> refers to the estimation method used: 'lmer' denotes sequential univariate multilevel estimation, 'Mplus' refers to multivariate Bayesian estimation (requires Mplus), and 'lm' represents fixed-effects estimation. The <strong>contemporaneous</strong> argument specifies the method for estimating the contemporaneous network, while <strong>temporal</strong> is used to estimate the temporal network.</p>

<p>First, we treat the data as longitudinal panel data with equal time intervals, without distinguishing between days and assessments, in order to estimate the network.</p>

<p>Visualize the three networks, where the <strong>rule</strong> can be set to 'and' or 'or'. 'And' means that an edge is drawn only if both nodes significantly predict each other, while 'or' means an edge is drawn as long as one of the nodes in the pair has a significant prediction. This way, we obtain the visualizations of the following three networks.</p>

<p>Next, when we consider the last measurement on day <strong>n</strong> and are unable to predict the first measurement on day <strong>n+1</strong>, we include both <strong>dayvar</strong> and <strong>beepvar</strong> in the estimation.</p>

<h2><strong>Node Centrality Estimation</strong></h2>
<p>Next, we estimate the centrality metrics for the nodes in each of the three networks.</p>

<p>That concludes the content of this article. If you have any questions, feel free to email me. If there are any errors in the text, please feel free to point them out.</p>
