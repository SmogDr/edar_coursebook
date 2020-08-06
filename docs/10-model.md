# Modeling {#model}
Text


> <span style="color: blue;"> "All models are wrong, but some are useful" - George Cox, pioneering statistician </span>

In this chapter we will discuss how models are conceptualized and "fit" to represent the data we have available. Models are potentially useful because they provide the basis for mathematical representation of data. This means that models can help you: 

  1. quantify patterns or trends in your data;
  2. extrapolate your data (i.e., to make a guess about an outcome outside the range of observed values);
  3. interpolate your data (i.e., to make a guess about an outcome within the range of observed values but not at a location where data has been observed)
  4. predict future outcomes;
  5. create a connection between *theory* (what you think should happen) and *observation* (what you actually see happen);




## Objectives

## Process Modeling

### Assumptions

## Fitting

> <span style="color: blue;"> "The purpose of models is not to fit the data but to sharpen the questions." - Samuel Karlim, mathematician and genomicist </span>

Although rare, the best models are those that advance your understanding of a phenomenon so well that you leave the model behind, because the questions have since changed. While we will not aspire to such great heights here, it is still worth understanding how models can be *fit* to data.  

Model construction, fitting, and validation could represent an entire semester (or more) of work.  Here, will will discuss only two introductory applications: distributional fitting and linear fitting.





## Linear Models

### Assumptions of Linear Regression

### Ordinary Least Squares

### Diagnostics

### Q-Q Plot (Quantile-Quantile) {#qqplot}

### Weighted Least Squares

### Deming Regression

## Probability Density Function {#pdf}  
The histogram is really an attempt to visualize the **probability density function** (pdf) for a distribution of univariate data.  The pdf is a mathematical representation of the likelihood of sampling a given value from the distribution, assuming a random draw.  Unlike the histogram (which bins data into groups), the pdf is a continuous function that can be solved at any datum.  A pdf can be expressed empirically (as a numeric approximation) or as an exact analytic equation (in the case normal, lognormal, and other *known* distributional forms).  

How to fit a distribution
### LOESS

### Calibration
