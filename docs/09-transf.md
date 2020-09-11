# Transformations and other Tricks {#transform}

This chapter provides an introduction to several common techniques to organize, transform, and/or manipulate univariate data. I use the word *manipulate* here with good reason; many of these techniques can be used inappropriately or lead to inadvertent consequences, so tread carefully!  That warning aside, these techniques are useful and often allow you to gain insight into *"what the data are trying to tell you"*.  

## Ch. 9 Objectives
This Chapter is designed around the following learning objectives. Upon completing this Chapter, you should be able to:  

- Define common approaches to data transformation  
- Explain the differences between *centering*, *rescaling*, and *standardizing* univariate data. 
- Stratify data into relevant groups for analysis
- Discuss the risks and potential approaches to handling outliers


## Transformation
The phrase *"Data Transformation"* means different things in different fields so we will define it here, in the statistical sense, as: ***applying one ore more mathematical operations to a variable*** (or in our case, all elements of numeric vector). You are undoubtedly familiar with data transformation, even if you haven't previously recognized it.  Some examples include:
- adding, subtracting, or multiplying a constant to all elements in a vector;
- taking the log, square root, or reciprocal of a variable;
- some combination of these approaches.

For example, your data are transformed whenever you convert temperature units from °F to °C, length units from inches to millimeters, or render a variable dimensionless (e.g., converting to units of percent, parts per million, or some other unitless value).  In addition to these "units transformations", we also conduct transformations to make the ***location*** and ***spread*** of data consistent, or to change the ***shape*** of a distribution of data (e.g., to reduce skewness or to linearize a variable). These latter approaches are often employed during multivariate analyses to help your variables "play nicely together" within the analytic framework you set forth. More on this to come.

<div class="rmdnote">
<p>There are two key facets to data transformation: <strong>justification</strong> and <strong>documentation</strong>. Whenever data are transformed, you must provide a good reason for doing so (hint: there are many good reasons and also a few bad ones). Furthermore, <strong>you must adequately document “how” and “why” the data were transformed</strong>, so that others can understand your work and reproduce your results if needed.</p>
</div>

### Centering
Centering means ***to subtract a constant from all values*** in a vector.  The **constant** to be subtracted could be an estimate of the data's central tendency, like the mean or median (hence the term *center*-ing), or it could be some residual value that you want to remove from all observations.  If you subtract the mean from every value of a vector, then the new vector will have a mean of 0; this can be useful when analyzing multiple variables (for example in a multivariate regression model), each of which has a different location.  Centering ***changes the location*** of a variable but ***spread stays the same***.

In engineering, the most common reason for centering data is not to subtract the mean or median value, but instead to "background subtract" or "zero" a set of univariate observations.  If you have ever used a digital scale to measure the weight of an object, you've probably noticed that when you turn it on, the scale does not always read zero.  Most of the time the instrument has a way to "re-zero" itself prior to use, but this is not always the case.  If your scale shows a mass of 298 g when nothing is placed on it, then you probably want to (1) document that value before taking measurements and (2) subtract a value of 298 from all subsequent weights that you perform with this scale.  That subtraction is an example of a "centering" transformation. 



<div class="figure" style="text-align: center">
<img src="./images/centering_anno.png" alt="Data are sometimes centered to remove spurious values"  />
<p class="caption">(\#fig:centering-anno)Data are sometimes centered to remove spurious values</p>
</div>

### Rescaling
**To rescale a variable means to divide (or multiply) all values by a constant**.  Whereas *centering* is an arithmetic transformation, the process of *rescaling* data is multiplicative.  Rescaling changes both the location and spread of the data.

Some reasons to rescale data include:  

- to show variables of different magnitudes together on the same axis of a plot;
- to make the spread of two or more variables similar (e.g., when conducting an analysis of variance or multivariate regression with variables that each have different scales);
- when you want to render a variable dimensionless (e.g., when converting to percentage units).

## Normalization and Standardization
In a generic sense, to ***"normalize"*** a variable means: **to transform that variable so that it can be compared, evaluated, or comprehended more easily.**  Normalization often involves a combination of centering and rescaling operations that not only change the location and spread of the data, but also the *meaning* of data.  Normalization is performed all the time. 
<div class="rmdwarning">
<p>Like <em>“transformation”</em>, the word “<em>normalization</em>” is defined differently in different fields, so be explicit when using this term in your work. To some, the word normalization means to “make a variable normally distributed”; to others, it means to “normalize a database” so that redundancies are eliminated.</p>
</div>

There are many types of normalization operations; we will discuss a few examples below.  

### Rate normalization
To perform a rate normalization is **to divide one variable into another**, often to report something in terms of a rate.  

  - Two cars can have the same range (how many miles they can travel on one tank of gas) but different rates of fuel efficiency (miles traveled per gallon)  
  - Two countries can have the same GDI (*gross domestic income*; the sum of all  wages earned) but widely different rates of "per capita" income (GDI/total population size). 

### Standardizing
Standardization is a special case of transformation/normalization where each value in a set of observations (or vector) is subtracted by some level (often the central tendency) and divided by the spread.  When working with normally distributed data, the standardization of variable x~i~ into z~i~ is:

     $z_{i} = \frac{x_{i}-\overline{x}}{\sigma_{x}}$

where the mean and standard deviation of x are $\overline{x}$ and $\sigma_{x}$, respectively. The *standardization* of a variable renders the mean to 0 and the standard deviation to 1.  This type of normalization is useful when you wish to study relationships between multiple variables, each with different scale. Note that standardization doesn't have to use  the mean and standard deviation as the centering and normalizing variables, but those are most common.

### Reducing Skewness
Sometimes your data are *not normally distributed*, even though you want them to be. In that case you still have options.  Some transformations, like the log, square root, or inverse, will make the spread of the data symmetric and approximately Gaussian. These transformations are commonly used to meet the needs (read: underlying assumptions) of many statistical models. We discuss model assumptions (and their evaluation) in the [modeling chapter](#model).



<div class="figure" style="text-align: center">
<img src="./images/reduce_skew_anno.png" alt="The log(x) transform is often used to reduce skewness in a variable."  />
<p class="caption">(\#fig:reduce-skew2)The log(x) transform is often used to reduce skewness in a variable.</p>
</div>
## Stratification
**To stratify a sample means: to divide the sample into subsets (aka strata).** 

Stratified analyses can be performed many ways; one particularly useful way is graphically, by creating a stratification plot.  A stratification plot is like any other visualization that you have created, except that the strata are identified (i.e., called out) through the use of color, lines, symbols, etc.  Let's use the following example to demonstrate:


A manufacturing operation is investigating the production of an expensive titanium alloy, where a 25% increase in the production yield of the alloy manufacturing process means the difference between profit and loss.  The materials group believes that sample purity should have an effect on the yield and for the past month, all three reactors at the plant have been producing alloys from Ti feedstock of varying purity. After one month, you decide to visualize the relationship between *process yield* and *sample purity* with a scatterplot, `geom_point()`.

<div class="figure" style="text-align: center">
<img src="./images/unstratified.png" alt="Yield vs. Purity from the plant's three reactors over one month" width="500" />
<p class="caption">(\#fig:stratify2)Yield vs. Purity from the plant's three reactors over one month</p>
</div>
Examination of Figure \@ref(fig:stratify2) reveals that, while there is considerable variation in the data, a clear relationship is not apparent between sample purity and process yield. However, it occurs to you that perhaps there is variation in results between the three different reactors in the plant.  Therefore, you decide to repeat this analysis ***stratifying the data by reactor type***.  In this case, you repeat the `ggplot()` call with an extra aesthetic of `color = reactor` in the scatterplot.



<div class="figure" style="text-align: center">
<img src="./images/stratified.png" alt="Yield vs. Purity over one month, stratified by reactor type" width="500" />
<p class="caption">(\#fig:stratify-png)Yield vs. Purity over one month, stratified by reactor type</p>
</div>
Figure \@ref(fig:stratify-png) tells a very different story! Now we can see that two relationships are clearly evident.  

  1. There is a definite inverse relationship between *sample purity* and the *process yield* and  
  2. For a given purity level, each reactor has a different output.  Clearly, there is something different about each of these reactors that merits further study...  
  
Figure \@ref(fig:stratify-png) raises a subtle, but very important, point: **data often have hierarchy** and that hierarchy may be influential. In this case, only when we account for the hierarchy (the reactor that produced each sample), do we see an important relationship arise.

*Hierarchical data* means data that can be ordered into different ranks (or levels). Students at a university might be ordered by their their college, their major, or the year in which they matriculated. Those ranks may be important when analyzing student data; if so, your analysis should account for that!

## Outliers and Censoring
Have you ever heard the phrase ***"don't make the exception the rule"***? 

I think this phrase means, *don't let your judgment be governed solely by outliers.*  Whether or not this is good advice probably depends on the situation, but I do think one should have the ability to detect ***when an outlier has leverage over a situation***. 

<div class="rmdnote">
<p>An <strong>outlier</strong> is an observation that lies an abnormal distance from other values in a random sample.</p>
</div>

### Detecting Outliers {#outliers}
Data visualization (like histograms, density plots, time-series, and boxplots) can be useful for detecting outliers, because such graphs make it clear that one or more observations *"don't seem to belong with the rest"*.  For example, let's create an artificial dataframe called `asthma.data`. Within this dataframe are two variables:  

  - `asthma.rate`: the percentage of kids with asthma in a given school district
  - `black.carbon`: the concentration of airborne black carbon (a type of air pollutant generated by incomplete combustion, similar to what you see exhausted from a diesel truck) measured outside of a school at the center of each district.


```r
asthma.data <- tibble(
  asthma.rate = c(8, 6, 12, 18, 9, 
                  8, 15, 14, 14, 10,
                  16, 9, 12, 15, 9),
  black.carbon = c(3.2, 2.5, 4.6, 6.1, 3.3, 
                   3.1, 6.1, 5.3, 4.5, 3.3, 
                   6.4, 3.7, 5.1, 6.8, 12)
)
```

The plot below shows two boxplots: one depicts `asthma.rate` for the 15 school districts; the other depicts `black.carbon`. I've circled the apparent outlier. **Outliers are easy to see in boxplots because they fall outside the whiskers.**

<div class="figure" style="text-align: center">
<img src="09-transf_files/figure-html/outlier-1-1.png" alt="Boxplots for two sets of data; the one on the right has an outlier." width="500" />
<p class="caption">(\#fig:outlier-1)Boxplots for two sets of data; the one on the right has an outlier.</p>
</div>

Data visualization is *useful* for detecting outliers, but not definitive.  Indeed, there are no definitive methods for detecting outliers in data (mostly because that process is too contextual).  That said, there are some guidelines.

**The "1.5 x IQR" rule** is what was used in the boxplot of Figure \@ref(fig:outlier-1).  

This rule states that outliers are **any data points more than 1.5 times the inter-quartile range away from the upper and lower quartiles**.  This means data that are:  

  - *greater than*: [0.75 quantile value] + 1.5*IQR
  - *less than*: [0.25 quantile value] - 1.5*IQR
  
For the `black.carbon` data, we could calculate these thresholds with the following:


```r
upper <- quantile(x = asthma.data$black.carbon,
                  probs = 0.75,
                  names = FALSE) + 
  1.5*IQR(x = asthma.data$black.carbon)

lower <- quantile(x = asthma.data$black.carbon,
                  probs = 0.25,
                  names = FALSE) - 
  1.5*IQR(x = asthma.data$black.carbon)

upper
```

```
## [1] 10.3
```

```r
lower
```

```
## [1] -0.9
```
Interestingly, this rule suggests that outliers on the lower end need to be negative. Is that even possible for air pollution concentrations? (hint: NO) Other  statistical approaches for detecting outliers include:  

* a threshold of standard deviations away from the mean (i.e., computing z-scores); 
* Grubbs’ test;
* Tietjen-Moore Test, and others.

In the end, however, common sense and your contextual knowledge of the data are usually more important here.

### Censoring outliers
What should we do with outliers in our data? The answer depends on context (*are these life and death data we're dealing with or just the size of tomatoes from your garden?*) and on the questions you are asking. Here's an example:

The black carbon boxplot (right side of \@ref(fig:outlier-1)) has a clear outlier that falls far outside of the rest of the data.  This might not seem like a big deal but take a look at what happens when we fit a regression line through the data <span style="color: orange;">with the outlier included</span> and <span style="color: blue;">without the outlier</span>. 
<div class="figure" style="text-align: center">
<img src="09-transf_files/figure-html/outlier-2-1.png" alt="Effect of an outlier on a linear model with n=15 data points." width="672" />
<p class="caption">(\#fig:outlier-2)Effect of an outlier on a linear model with n=15 data points.</p>
</div>
Examination of Figure \@ref(fig:outlier-2) suggests that the outlier is having a strong effect on the relationship between air pollution and childhood asthma. If the censored data are correct, we have just detected a strong correlation between asthma rates and air pollution. If the uncensored data are correct then the association is positive but very weak. So how do we handle this apparent outlier?  

Unfortunately, this sort of conundrum happens more often than we might like in the real world. *My advice would be to collect more data to support/discount the outlier. If that isn't possible (and no other explanation was forthcoming), I would probably show the analysis* ***both ways***.

<div class="rmdwarning">
<p><strong>Censoring data is dangerous business.</strong> If you are going to censor an outlier, make sure to document <strong>how</strong> you discovered/defined the outlier, <strong>why</strong> you believe it should be censored, and <strong>“whether or not that censoring had an effect on your results/conclusions”</strong>. Then make sure to broadcast your thinking to everyone who comes in contact with your analysis If you try to hide outliers, you are being unethical and setting yourself up for disaster.</p>
</div>

## Ch-9 Exercises

## Ch-9 Homework

