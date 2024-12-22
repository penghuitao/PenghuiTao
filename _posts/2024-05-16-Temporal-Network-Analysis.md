---
layout: page
title:  "Temporal Network Analysis"
subtitle: ""
date:   2024-05-16 21:21:21 +0530
categories: ["Longitudinal Analysis"]
---

<h2><strong>Preface</strong></h2>
<p>Network analysis has become increasingly common in the field of psychology, especially temporal network analysis for intensive longitudinal data, which has gained a lot of academic attention. In this article, I will provide a brief introduction to what temporal network analysis is and how to implement it in R. These codes may not be the most concise or optimal, but I can assure you they work properly (assuming you use them correctly in R).

<h2><strong>What is Temporal Network Analysis?</strong></h2>
<p>In the field of psychological research, network analysis methods were first applied to the study of psychological disorders (see Borsboom, 2017). However, so far, most psychological research involving network analysis has used cross-sectional networks. These studies primarily focus on the interactions between variables at a single point in time. However, most psychological variables evolve over time. Therefore, examining the relationships between variables longitudinally is an important direction for the development of network methods (Blanchard et al., 2022; Bringmann et al., 2021). As is well known, longitudinal data in psychological research can be divided into two types: developmental process data and stable process data. These two types of data are obtained through different research designs and measurement methods, and their main differences can be seen in the table below. For panel data with 2-3 waves of tracking, cross-lagged network analysis is commonly used. For panel data with more than 5 waves of tracking and intensive longitudinal data, temporal network analysis is commonly employed.</p>

<p>Temporal network analysis methods are built upon the Vector Autoregression (VAR) model. Simply put, the "vector" in the VAR model refers to multiple features and is an extension of the univariate autoregressive model. The VAR model consists of a set of regression equations in which all variables are treated as endogenous. Therefore, they can serve both as independent and dependent variables. This allows VAR analysis to be conducted without prior assumptions about the direction of associations between variables. Through this method, we can uncover the cross-temporal dynamic relationships between multiple variables. Additionally, the residuals of the variables can further be used to estimate the correlation between variables at the same time point.</p>
<p>Based on the results of the VAR model, temporal network analysis can yield three types of network outcomes:</p>
<ol>
  <li><strong>Temporal network</strong>. In a temporal network, edges represent the predictive ability of a variable at a previous time point on the node at the next time point, after controlling for all other variables at the previous measurement. For example, as shown in the diagram, we can observe that sadness, intimacy with others, experiential avoidance, self-esteem, and paranoid beliefs all have autoregressive effects. Intimacy with others negatively predicts paranoia at the next time point, sadness positively predicts experiential avoidance, and experiential avoidance, in turn, positively predicts self-esteem at the next time point.</li>
<img src="{{ '/assets/img/Temporal_network.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
  <li><strong>Contemporaneous network</strong>. In a contemporaneous network, edges represent the associations between variables within the same time frame, after controlling for temporal associations. Several relationships stand out clearly, such as the positive correlation between paranoid beliefs and sadness, indicating that higher paranoid beliefs are associated with higher levels of sadness at the same time. Similarly, sadness is negatively correlated with both self-esteem and intimacy with others, meaning that the higher the sadness, the lower the self-esteem and intimacy with others. Additionally, self-esteem and intimacy with others are positively correlated. Finally, experiential avoidance shows no significant correlation with any other variable.</li>
<img src="{{ '/assets/img/Contemporaneous_network.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
  <li><strong>Between-subject network</strong>. In the between-subject network model, edges represent the correlations between the average levels of variables within individuals, after accounting for the remaining variables in the network. It can be observed that the average level of intimacy with others is negatively correlated with the average level of paranoid beliefs and positively correlated with the average level of self-esteem. The average level of sadness is negatively correlated with the average level of self-esteem and positively correlated with the average level of experiential avoidance.</li>
<img src="{{ '/assets/img/Between-subject_network.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
</ol>

<h2><strong>Implementation in R</strong></h2>
<p>In R, we use the mlVAR package to implement temporal network analysis. Here, we use data from the study <em>A Temporal Network Approach to Paranoia: A Pilot Study</em> (Contreras et al., 2020) as an example.</p>

<h2><strong>Data Introduction</strong></h2>
<p>The study used an experience sampling design with temporal variation, where participants received a total of 10 questionnaire assessments each day from 9 AM to 10 PM over the course of 7 days. The longitudinal data is shown in the figure, where the subject variable represents the participant ID, the day variable represents the day number, and the beep variable represents the number of the questionnaire assessment.</p>
<img src="{{ '/assets/img/1_23_a.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<h2><strong>Network Estimation</strong></h2>
<p>To estimate the network, the mlVAR() function is used. The data argument refers to the longitudinal data shown above, vars includes the variables to be analyzed, idvar represents the participant ID variable, lag is the time lag, dayvar is the day variable, and beepvar indicates the number of the assessment. The estimator refers to the estimation method used: 'lmer' denotes sequential univariate multilevel estimation, 'Mplus' refers to multivariate Bayesian estimation (requires Mplus), and 'lm' represents fixed-effects estimation. The contemporaneous argument specifies the method for estimating the contemporaneous network, while temporal is used to estimate the temporal network.</p>
<img src="{{ '/assets/img/1_23_b.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<p>First, we treat the data as longitudinal panel data with equal time intervals, without distinguishing between days and assessments, in order to estimate the network.</p>
<img src="{{ '/assets/img/1_23_c.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<p>Visualize the three networks, where the rule can be set to 'and' or 'or'. 'And' means that an edge is drawn only if both nodes significantly predict each other, while 'or' means an edge is drawn as long as one of the nodes in the pair has a significant prediction. This way, we obtain the visualizations of the following three networks.</p>
<img src="{{ '/assets/img/1_23_d.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_e.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_f.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%;、 max-width: 800px;">
<img src="{{ '/assets/img/1_23_g.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<p>Next, when we consider the last measurement on day n and are unable to predict the first measurement on day n+1, we include both dayvar and beepvar in the estimation.</p>

<img src="{{ '/assets/img/1_23_h.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_i.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_j.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_k.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">

<h2><strong>Node Centrality Estimation</strong></h2>
<p>Next, we estimate the centrality metrics for the nodes in each of the three networks.</p>
<img src="{{ '/assets/img/1_23_m.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<img src="{{ '/assets/img/1_23_n.png' | prepend: site.baseurl }}" id="about-img" style="width: 50%; max-width: 800px;">
<p>That concludes the content of this blog. If you have any questions, feel free to email me.</p>
<p>Contreras, A., Valiente, C., Heeren, A., & Bentall, R. (2020). A Temporal Network Approach to Paranoia: A pilot study. <i>Frontiers in Psychology, 11</i>. <a href="https://doi.org/10.3389/fpsyg.2020.544565">https://doi.org/10.3389/fpsyg.2020.544565</a>
<br>Borsboom, D. (2017). A network theory of mental disorders. <i>World Psychiatry, 16</i>(1), 5–13. <a href="https://doi.org/10.1002/wps.20375">https://doi.org/10.1002/wps.20375</a>
<br>Blanchard, M. A., Contreras, A., Kalkan, R. B., & Heeren, A. (2022). Auditing the research practices and statistical analyses of the group-level temporal network approach to psychological constructs: A systematic scoping review. <i>Behavior Research Methods, 55</i>(2), 767–787. <a href="https://doi.org/10.3758/s13428-022-01839-y">https://doi.org/10.3758/s13428-022-01839-y</a>
<br>Bringmann, L. F., Albers, C., Bockting, C., Borsboom, D., Ceulemans, E., Cramer, A., Epskamp, S., Eronen, M. I., Hamaker, E., Kuppens, P., Lutz, W., McNally, R. J., Molenaar, P., Tio, P., Voelkle, M. C., & Wichers, M. (2021). Psychopathological networks: Theory, methods and practice. <i>Behaviour Research and Therapy, 149</i>, 104011. <a href="https://doi.org/10.1016/j.brat.2021.104011">https://doi.org/10.1016/j.brat.2021.104011</a></p>
