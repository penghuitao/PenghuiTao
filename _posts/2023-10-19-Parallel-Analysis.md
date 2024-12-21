---
layout: page
title:  "Parallel Analysis"
subtitle: ""
date:   2023-10-19 21:01:21 +0530
categories: ["Factor Analysis"]
---


<h2><strong>Introduction to Parallel Analysis</strong></h2>
<p>Today, I'm introducing a method that I think isn't very useful, but is still frequently used by many people: Parallel Analysis.</p>

<p>As we all know, determining the number of factors in factor analysis is a very challenging issue. In addition to ensuring that each common factor has a meaningful interpretation, statisticians have proposed various methods. Common approaches include extracting factors based on eigenvalues greater than 1 or using the scree plot steep slope criterion. The method I’m introducing today is Parallel Analysis.</p>

<p>Parallel Analysis is not a new method; it was proposed by Horn in 1965. However, its application is not as widespread as the other two methods mentioned earlier. The reason for this is that its computation is relatively more complex. Let’s look at its basic idea. This method suggests that even if variables are completely uncorrelated, factor analysis on their data matrix might still randomly extract some "pseudo" factors due to random fluctuations in the data. If the factors we want to extract are real, their information should be significantly higher than the pseudo-factors extracted from random samples. The idea is to generate a random data matrix that has the same structure (variables and sample size) as the real data, but with random values. If we perform factor analysis on this random data matrix, the result should show entirely random fluctuations. If the factors in the actual data are meaningful, their eigenvalues should be much higher than those of the random data. By comparing the scree plots of the actual data and the random data eigenvalues, we can clearly see which factors should be retained.</p>

<p>Clearly, the logic behind this analysis is easy to understand and quite convincing.</p>

<h2><strong>So why do I say this method isn’t very useful? </strong></h2>

<p>Because although the theoretical basis of Parallel Analysis is sound, its practical value is limited. First, the eigenvalue >1 criterion and the scree plot have already done a good job, so Parallel Analysis doesn’t contribute much more. Secondly, the results of Parallel Analysis are unstable. The larger the sample size and the fewer the measured variables, the fewer the factors Parallel Analysis suggests. Thirdly, it doesn’t accurately estimate the number of factors. In general, Parallel Analysis tends to suggest too many factors, so its recommended number of factors is often taken as an upper limit. The most important point, however, is that the core requirement of factor analysis is the interpretability of the factors. Factors that cannot be interpreted professionally cannot be retained.</p>

<p>However, let’s still go over its implementation steps and Mplus code.</p>

<h2><strong>Steps of Parallel Analysis</strong></h2>
<ol>
  <li>Generate a random data matrix that has the same number of variables and sample size as the real data.</li>
  <li>Calculate the eigenvalues of this random data matrix and compute the average eigenvalue.</li>
  <li>By comparing the scree plots (or eigenvalue curves) of the real data and the random matrix's average eigenvalue curve, we can identify the point where the two curves intersect. This point determines the maximum number of factors to extract.</li>
</ol>

<p>If the eigenvalues of the real data are above the random matrix’s average eigenvalue curve, those factors should be retained; otherwise, they should be discarded. Simply put, we generate 100 random matrices with the same number of variables and cases as our actual data, then compare the average eigenvalues of these random matrices to the eigenvalues of our actual data.</p>

<h2><strong>Mplus Code for Parallel Analysis</strong></h2>
<pre>
TITLE: PARALLEL ANALYSIS;

DATA: FILE = EFA.dat; ! Data file

VARIABLE: NAMES ARE X1-X31; ! Variable names
          USEVARIABLES ARE X1-X31; ! Variables to be used
          MISSING ARE ALL (99); ! Define missing values

ANALYSIS: PARALLEL = 100; ! Number of random matrices to generate (default is 100)
          TYPE = EFA 1 7; ! Factor extraction range (1 to 8)

PLOT: TYPE IS PLOT2; ! Request scree plot
</pre>

<h2><strong>Result Interpretation</strong></h2>
<p>The eigenvalues of the three factors in the real data fall on the average eigenvalue curve of the random data matrix, so we retain these three factors.</p>
