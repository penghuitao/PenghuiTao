
<h3>What is Confirmatory Factor Analysis (CFA)?</h3>
<p>Confirmatory Factor Analysis (CFA) is the opposite of Exploratory Factor Analysis (EFA). It involves defining specific factors or constructs of interest (e.g., loyalty, satisfaction) based on literature, and then using survey items to measure these constructs (e.g., loyalty could be measured by purchase frequency or willingness to recommend). The primary goal of CFA is to test whether the items indeed reflect the construct as hypothesized, making CFA a tool for confirming theoretical models.</p>

<p>In CFA, each factor is related only to its own measurement items and not to the items of other factors. For example, when reporting a Structural Equation Model (SEM), each item is associated with only its corresponding factor, and no relationships are assumed between factors. This means that when drawing the CFA model, each item is clearly linked only to its own factor, with no correlations with items of other factors. This independence helps confirm whether the items accurately measure the predefined constructs.</p>

<h3>How to Use Confirmatory Factor Analysis?</h3>

<p><strong>1. Model Specification:</strong> Before performing CFA, it is essential to define the relationships between observed variables (items) and factors/latent variables, the relationships between the factors/latent variables themselves, and the relationships between error terms.</p>
<p>This includes several questions:</p>
<ul>
  <li>How many factors/dimensions are there?</li>
  <li>Which observed variables are associated with each factor?</li>
  <li>How do the factors relate to each other (if there are multiple factors)?</li>
  <li>How do the error terms relate to each other?</li>
  <li>The unidimensionality of observed variables: An observed variable should measure only one latent variable and not overlap with another latent variable (i.e., no cross-loadings).</li>
</ul>

<p><strong>2. Model Identification:</strong> Before parameter estimation, the model must be identified, meaning we need to check if the model parameters have a unique solution.</p>
<p><strong>3. Model Estimation (Parameter Estimation):</strong> CFA reflects how well the theoretical model matches the data by comparing the population covariance matrix derived from the model with the sample covariance matrix.</p>

<p><strong>4. Model Evaluation:</strong> Model evaluation assesses how well the model fits the data using fit indices. It is important to consider multiple fit indices when evaluating a model.</p>

<p><strong>5. Model Modification:</strong> If the model does not fit the data well, it may need to be modified. Model modification should be guided by theoretical justification, not just software output.</p>

<h3>How to Implement CFA in Mplus?</h3>

<h4>1. First-Order CFA:</h4>
<pre>
TITLE: CFA;    
      ! This syntax file performs Confirmatory Factor Analysis

DATA: FILE IS 123.dat;    
      ! The data file is named 123 (ensure the data file and syntax file are in the same folder)

VARIABLE: NAMES ARE x1-x31;    
          ! The data file contains 31 variables, named x1 to x31
          USEVARIABLES ARE x1-x31;    
          ! These 31 variables are used in the analysis, corresponding to the 31 questionnaire items
          MISSING = ALL (99);    
          ! Missing values are coded as 99 and excluded from the analysis

ANALYSIS: ESTIMATOR = MLR;    
          ! Use Robust Maximum Likelihood Estimation (MLR), which can be adjusted based on the data characteristics

MODEL: F1 BY x1 x4 x7 x10 x13 x16 x19 x22 x25 x28;    
       F2 BY x2 x5 x8 x11 x14 x17 x20 x23 x26 x29 x31;    
       F3 BY x3 x6 x9 x12 x15 x18 x21 x24 x27 x30;    
       ! Define latent factors (F1, F2, F3) using the observed variables (e.g., x1, x4, x7, etc.)

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL;    
        ! Output sample statistics, standardized values, modification indices, and parameter confidence intervals
</pre>

<h4>2. Second-Order CFA:</h4>
<pre>
TITLE: CFA;    

DATA: FILE IS 123.dat;  
VARIABLE: NAMES ARE x1-x31;    
          USEVARIABLES ARE x1-x31;    
          MISSING = ALL (99);  

ANALYSIS: ESTIMATOR = MLR;

MODEL: F1 BY x1 x4 x7 x10 x13 x16 x19 x22 x25 x28;    
       F2 BY x2 x5 x8 x11 x14 x17 x20 x23 x26 x29 x31;    
       F3 BY x3 x6 x9 x12 x15 x18 x21 x24 x27 x30;    
       F BY F1 F2 F3;    
       ! The second-order factor F influences the first-order factors F1, F2, and F3

OUTPUT: SAMPSTAT STDYX MOD CINTERVAL;
</pre>

