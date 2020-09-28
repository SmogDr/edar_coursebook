# Appendix: Reference Distributions {#dist}
In this chapter I discuss a handful of reference distributions that you may encounter while working thorugh this course. I don't go into great detail on any of these distributions (or their mathematical structure) because smarter, better, and more authoritative descriptions can be found elsehwere in reference texts or online. This is the $1 tour.  



## Uniform Distribution {#unif_dist}
The uniform distribution describes a situation where all obervations have an equal probability of occurrence. Examples of uniform distributions include: the outcome of a single  roll of a 6-sided die, or the flip of a coin, or the chance of picking a spade, heart, diamond, or club from a well-shuffled deck of cards.  In each of these cases, all potential outcomes have an equal chance of occuring.  A uniform distribition may be specified simply by setting the range of possible outcomes (i.e., a minimum, maximum, and anything in between). The "in-between" part also lets you specify whether you want to allow outcome values that are continuous (like from a random-number generator), integers (like from a dice roll), or some other format (like a binary 0 or 1; heads or tails).  

Below, we create a [probability density function](#pdf)  for the first roll of a six-sided die; this is a *discrete uniform distribition* since we only allow integers to occur.  A uniform distribution is appropriate here because any of the numbers between 1 and 6 has equal probability of being rolled.  Notice the shape of the histogram...flat.


```r
#create a uniform distribition for the first roll of a 6-sided die
six_sided <- tibble(
  rolls = ceiling(runif(10e4, min=1, max=7))
)

#create a histogram of the probability density for a uniform distribution 
ggplot(data = six_sided, aes(x = rolls)) +
  geom_histogram(
      breaks = seq(1,7,1), 
      fill = "grey",
      color = "white") +
  xlab("Number") +
  scale_x_continuous(breaks = c(0.5, 1.5, 2.5, 3.5, 4.5, 5.5, 6.5),
                     labels = as.factor(seq(0,6,1))) +
  theme_bw(base_size = 12)
```

<div class="figure">
<img src="13-distributions_files/figure-html/unif-dist-1.png" alt="Outcome probability for the first roll of a 6-sided die" width="672" />
<p class="caption">(\#fig:unif-dist)Outcome probability for the first roll of a 6-sided die</p>
</div>
  
  
### Characteristic Plots: Uniform Distribution
Below, we show the cumulative distribution plot and probability density function for a uniform distribution between 0 and 1.
<div class="figure">
<img src="13-distributions_files/figure-html/unif-dist-plots2-1.png" alt="Characteristic Plots for a Uniform Distribution" width="672" />
<p class="caption">(\#fig:unif-dist-plots2)Characteristic Plots for a Uniform Distribution</p>
</div>

## Normal Distribution {#normal_dist}
The normal distribution arises from phenomena that tend to have *additive* variability. By "additive", I mean that the outcome (or variable of interest) tends to vary in a +/- fashion from one observation to the next. Lots of things have additive variability: the heights of 1^st^ graders, the size of pollen grains from a tree or plant, the variation in blood pressure across the population, or the average temperature in Fort Collins, CO for the month of June.  

Let's examine what *additive variability* looks like using the 6-sided dice mentioned above.  Although a dice roll has a uniform distribution of possible outcomes (rolling a 1,2,3,4,5, or 6), the variability associated with adding up the sum of three or more dice creates a normal distribution of outcomes.  If we were to roll four, 6-sided dice and sum the result (getting a value between 4 and 24 for each roll), and then repeat this experiment 10,000 times, we see the distribution shown below.  The smooth line represents a fit using a normal distribution - a pretty nice fit considering that we are working with a discrete (integer-based) dataset!
<div class="figure">
<img src="13-distributions_files/figure-html/normal1-1.png" alt="A Normal Distribution" width="672" />
<p class="caption">(\#fig:normal1)A Normal Distribution</p>
</div>

### Normal Distribution: Characteristic Plots

Unlike the uniform distribution, the normal distribution is not specified by a range (it doesn't have one).  The normal distribution is specified by a *central tendancy* (a most-common value) and a measure of data's *dispersion* or spread (a standard deviation). A normal distribution is symmetric, meaning that the spread of the data is equal on each side of the central tendency.  This symmetry also means that the mode (the most common value), the median (the 50^th^ percentile or 0.5 quantile) and the mean (the average value) are all equal. A series of normal distributions of varying dispersion is shown in the panels below.

<div class="figure" style="text-align: center">
<img src="13-distributions_files/figure-html/normal2-1.png" alt="Characteristic Plots for a Normal Distribution" width="576" />
<p class="caption">(\#fig:normal2)Characteristic Plots for a Normal Distribution</p>
</div>

## Log-normal Distribution {#log_normal_dist}
Multiplicative variation is what gives rise to a "log-normal" distribution: a special type of skewed data.  

Let's create two normal distributions for variables 'a' and 'b':  


```r
#create two variables that are normally distributed
normal_data <- tibble(a = rnorm(n=1000, mean = 15, sd = 5),
                   b = rnorm(n=1000, mean = 10, sd = 3))
```

Individually, we know that these data are normally distributed (because we created them that way), but what does the distribution look like if we add these two variables together?


```r
#add those variables together and you get a normal distribution
normal_data %>% mutate(c = a + b) -> normal_data

ggplot2::ggplot(data = normal_data) + 
  geom_histogram(aes(c),
                 bins = 30,
                 fill = "navy",
                 color = "white") +
  xlab("Sum of a + b") +
  theme_minimal(base_size = 12)
```

<div class="figure" style="text-align: center">
<img src="13-distributions_files/figure-html/a-b-histogram-1.png" alt="The Sum of Two Normally Distributed Variables" width="672" />
<p class="caption">(\#fig:a-b-histogram)The Sum of Two Normally Distributed Variables</p>
</div>

```r
#ggsave("./images/hist_a_b.png", dpi = 150)
```
*Answer: still normal*.  Since all we did here was add together two normal distributions, we simply created a third (normal) distribution with more additive variability.  

What happens, however, if we multiply together a series of normally distributed variables?  

```r
#multiply together three normal variables
normal_data %>% 
  mutate(d = sample(a*b*c, 1000)) -> log_data

ggplot2::ggplot(data = log_data) + 
  geom_histogram(aes(d),
                 bins = 30,
                 fill = "orange",
                 color = "white") +
  xlab("a * b * c") +
  theme_minimal(base_size = 12) 
```

<div class="figure" style="text-align: center">
<img src="13-distributions_files/figure-html/a-b-lognormal-1.png" alt="The Product of Three Normally Distributed Variables Multiplied Together" width="672" />
<p class="caption">(\#fig:a-b-lognormal)The Product of Three Normally Distributed Variables Multiplied Together</p>
</div>

```r
#ggsave("./images/hist_skew_out.png", dpi = 150)
```
*Answer: the additive variability becomes **multiplicative variability**, which leads to a skewed (in this case, log-normal) distribution.*

Multiplicative (or log-normal) variability arises when the mechanism(s)
controlling the variation of `x` are multipliers (or divisors) of `x`. Many
real-world phenomena create multiplicative variability in observed data: the
strength of a WiFi signal at different locations within a building, the
magnitude of earthquakes measured at a given position on the Earth's surface,
the size of rocks found in a section of a riverbed, or the size of particles
found in air. All of these phenomena tend to be governed by multiplicative
factors. In other words, all of these observations are controlled by mechanisms
that suggest $x = a * b * c$ not $x = a\cdot b\cdot c$.



