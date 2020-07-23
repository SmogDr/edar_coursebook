# Transformations and other Tricks {#eda3}

This chapter provides an introduction to several common techniques to organize, transform, and/or manipulate univariate data. I use the word *manipulate* here with good reason; many of these techniques can be used inappropriately or lead to inadvertent consequences, so tread carefully!  That warning aside, these techniques are useful and often allow you to gain insight into *"what the data are trying to tell you"*.  

## Objectives
This Chapter is designed around the following learning objectives. Upon completing this Chapter, you should be able to:  

- Define common approaches to data transformation  
- Explain the differences between *centering*, *rescaling*, and *standardizing* univariate data. 


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
<img src="./images/centering_anno.png" alt="Data are sometimes centered to remove spurious values" width="640" />
<p class="caption">(\#fig:centering-anno)Data are sometimes centered to remove spurious values</p>
</div>

### Rescaling
**To rescale a variable means to divide (or multiply) all values by a constant**.  Whereas *centering* is an arithmetic transformation, the process of *rescaling* data is multiplicative.  Rescaling changes both the location and spread of the data.

Some reasons to rescale data include:  

- to show variables of different magnitudes together on the same axis of a plot;
- to make the spread of two or more variables similar (e.g., when conducting an analysis of variance or multivariate regression with variables that each have different scales);
- when you want to render a variable dimensionless (e.g., when converting to percentage units).

## Normalization and Standardization
In a generic sense, to ***"normalize"*** a variable means: **to transform that variable so that it can be compared, evaluated, or comprehended more easily.**  Normalization often involves a combination of centering and rescaling operations that not only change the location and spread of the data, but also the *meaning* of data.  Normalization is used all the time. 
<div class="rmdwarning">
<p>Like <em>“transformation”</em>, the word “<em>normalization</em>” is defined differently in different fields, so be explicit when using this term in your work. To some, the word normalization means to “make a variable normally distributed”; to others, it means to “normalize a database” so that redundancies are eliminated.</p>
</div>

There are many types of normalization operations; we will discuss a few examples here:  

### Rate normalization
To perform a rate normalization is **to divide one variable into another**, often to report something in terms of a rate.  

  - Two cars can have the same range (how many miles they can travel on one tank of gas) but different rates of fuel efficiency (miles traveled per gallon)  
  - Two countries can have the same GDI (*gross domestic income*; the sum of all  wages earned) but widely different rates of "per capita" income. 

### Standardizing
Standardization is a special case of transformation/normalization where each value in a set of observations (or vector) is subtracted by some level (often the central tendency) and divided by the spread.  When working with normally distributed data, the standardization of variable x_i into z_i is:

     $z_{i} = \frac{x_{i}-\overline{x}}{\sigma_{x}}$

where the mean and standard deviation of x are $\overline{x}$ and $\sigma_{x}$, respectively. The *standardization* of a variable renders the mean to 0 and the standard deviation to 1.  This type of normalization is useful when you wish to study relationships between multiple variables, each with different scale.

### Reducing Skewness
Sometimes your data are *not normally distributed*, even though you want them to be. In that case you still have options.  Some transformations, like the log, square root, or inverse, will make the spread of the data symmetric and approximately Gaussian. These transformations are commonly used to meet the needs (read: underlying assumptions) of many statistical models.



<div class="figure" style="text-align: center">
<img src="./images/reduce_skew_anno.png" alt="The log(x) transform is often used to reduce skewness in a variable." width="640" />
<p class="caption">(\#fig:reduce-skew2)The log(x) transform is often used to reduce skewness in a variable.</p>
</div>
## Stratification
**To stratify a sample means: to divide the sample into subsets (aka strata).** 

Stratified analyses can be performed many ways; one particularly useful way is graphically, by creating a stratification plot.  A stratification plot is like any other visualization that you have created, except that the strata are identified (i.e., called out) through the use of color, lines, symbols, etc.  Let's use the following example to demonstrate:


```
## Saving 7 x 5 in image
```
A manufacturing operation is investigating the production of an expensive titanium alloy, where a 25% increase in the production yield of the alloy manufacturing process means the difference between profit and loss.  The materials group believes that sample purity should have an effect on the yield and for the past month, all three reactors at the plant have been producing alloys from Ti feed stock of varying purity. After one month, you decide to visualize the relationship between *process yield* and *sample purity* with a scatterplot, `geom_point()`.


```r
knitr::include_graphics("./images/unstratified.png")
```

<div class="figure" style="text-align: center">
<img src="./images/unstratified.png" alt="Yield vs. Purity from 3 reactors over one month" width="1050" />
<p class="caption">(\#fig:stratify2)Yield vs. Purity from 3 reactors over one month</p>
</div>
Examination of Figure \@ref(fig:stratify2) reveals that, while there is considerable variation in the data, a clear relationship is not apparent between sample purity and process yield. However, it occurs to you that perhaps there is variation in results between the three different reactors in the plant.  Therefore, you decide to repeat this analysis ***stratifying the data by reactor type***.  In this case, you repeat the `ggplot()` call with an extra aesthetic of `color = reactor` in the scatterplot.


```r
stratified <- ggplot(data = strat.data,
       aes(y = yield2, 
           x = purity,
           color = reactor,
           stroke = 1)) +
  geom_point(size = 2,
             shape = 1) +
  labs(color = "Reactor",
       x = "Sample Purity, %",
       y = "Process Yield, kg") +
  theme_classic() +
  theme(legend.position = c(0.85, 0.8),
        legend.box.background = element_rect(colour = "black")) 

ggsave("./images/stratified.png")
```

```
## Saving 7 x 5 in image
```


```r
knitr::include_graphics("./images/stratified.png")
```

<div class="figure" style="text-align: center">
<img src="./images/stratified.png" alt="Yield vs. Purity from 3 reactors over one month" width="1050" />
<p class="caption">(\#fig:stratify3)Yield vs. Purity from 3 reactors over one month</p>
</div>
Figure \@ref(fig:stratify3) tells a very different story! Now we can see that two relationships are clearly evident.  

  1. There is a definite inverse relationship beteen *sample purity* and the *process yield* and  
  2. For a given purity level, each reactor has a different output.  Clearly, there is something different about each of these reactors that merits further study...  
  
## Censoring and Truncating
