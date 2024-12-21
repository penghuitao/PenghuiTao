


<p>In addition to the parallel mediation model, another common model with two mediator variables is the chain mediation model. In this model, there is a temporal sequence between the two mediator variables. As shown in the figure below, the mediator variable M1 comes first, followed by M2.</p>

<h2><strong>Observed Variable Chain Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL; 

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: Y M1 M2 ON X; ! Paths from X to M1, M2, and Y
       Y ON M1 M2; ! Paths from M1 and M2 to Y
       M2 ON M1; ! Path from M1 to M2

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Latent Variable Chain Mediation Model in Mplus</strong></h2>
<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variable names
          MISSING=ALL(99); ! Define missing values
          USEVARIABLE ARE X1-X4 A1-A4 B1-B4 Y1-Y4; ! Variables used in this analysis

ANALYSIS: ESTIMATOR=MLR; ! Estimation method, other methods like ML can be chosen based on data characteristics

MODEL: A BY X1-X4; 
       B BY A1-A4;
       C BY B1-B4;
       D BY Y1-Y4; ! Define four latent variables using BY
       D B C ON A; ! Paths from A to B, C, and D
       D ON B C; ! Paths from B and C to D
       C ON B; ! Path from B to C

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h2><strong>Mediation Effect Testing</strong></h2>
<h3>The Bootstrap method is used to test the mediation effect.</h3>
<p>Using the <code>MODEL CONSTRAINT</code> command, we can define the mediation effects, but this method only provides unstandardized values after running. Below is an example:</p>

<pre>
TITLE: MEDIATION MODEL; 

DATA: FILE IS 12.dat; ! Data source

VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
MISSING=ALL(99); ! Define missing values
USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: BOOTSTRAP=2000; ! Use Bootstrap method for mediation effect testing, sample size can be 1000/2000/5000 based on literature support

MODEL: Y ON X (cdash); ! Path from X to Y named as c'
Y ON M1 (b1); ! Path from M1 to Y named as b1
Y ON M2 (b2); ! Path from M2 to Y named as b2
M1 ON X (a1); ! Path from X to M1 named as a1
M2 ON X (a2); ! Path from X to M2 named as a2
M2 ON M1(d); ! Path from M1 to M2 named as d

MODEL CONSTRAINT:
NEW(a1b1 a2b2 a1db2 TOTALIND TOTAL); ! Define new coefficients
a1b1 = a1*b1; ! Indirect effect of X through M1 on Y
a2b2 = a2*b2; ! Indirect effect of X through M2 on Y
a1db2 = a1*d*b2; ! Indirect effect of X through M1 and M2 on Y
TOTALIND = a1*b1 + a2*b2 + a1*d*b2; ! Total indirect effect of X on Y
TOTAL = a1*b1 + a2*b2 + a1*d*b2 + cdash; ! Total effect of X on Y = indirect + direct

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>

<h3>Using the <code>MODEL INDIRECT</code> command, this method generates both unstandardized and standardized values and is simpler. It is recommended to use this method.</h3>

<pre>
TITLE: MEDIATION MODEL;

DATA: FILE IS 12.dat; ! Data source
VARIABLE: NAME ARE X M1 M2 Y; ! Variable names
           MISSING=ALL(99); ! Define missing values
           USEVARIABLE ARE X M1 M2 Y; ! Variables used in this analysis

ANALYSIS: BOOTSTRAP=2000; ! Use Bootstrap method for mediation effect testing, sample size can be 1000/2000/5000 based on literature support

MODEL: Y ON X; 
       Y ON M1; 
       Y ON M2; 
       M1 ON X; 
       M2 ON X; 
       M2 ON M1; 
       
MODEL INDIRECT: Y IND X; ! Computes indirect effects of X through M1 on Y
                               ! Computes indirect effects of X through M2 on Y
                               ! Computes indirect effects of X through both M1 and M2 on Y
                               ! Computes total indirect effect

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL; ! Output sample statistics, standardized values, modification indices, confidence intervals
</pre>
