---
title: "Class 08: Inference for Correlation; Multiple Means"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Understand the correlation between two variables.
- Know how to use the three hypothesis tests for testing correlation between two variables.
- Use the One-way ANOVA and Kruskal-Wallis rank sum test to extend the T-Test to 3 or more groups.

## Correlation

You have probably heard the term *correlation*, but may not have ever seen a
formal definition. If we have pairs of observations from two variables x and y,
the correlation between them is written by the following seemingly complex formula:

$$ cor(x, y) = \frac{(x_1 - \bar{x})(y_1 - \bar{y}) + \cdots + (x_n - \bar{x})(y_n - \bar{y})}{\sqrt{\left[(x_1 - \bar{x})^2 + \cdots + (x_n - \bar{x})^2 \right] \cdot \left[(y_1 - \bar{y})^2 + \cdots + (y_n - \bar{y})^2 \right]}} = \frac{\sum_i (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_i (x_i - \bar{x})^2 \cdot \sum_i(y_i - \bar{x})^2 }}$$

I do not want you to worry too much about the correlation equation. Rather,
take away the following facts about this value:

1. The correlation is always between -1 and 1
2. The correlation of two unrelated variables is 0.
3. The correlation is positive if y "tends to be larger than its mean" whenever x is larger than its mean.
4. The correlation is negative if y "tends to be less than its mean" whenever x is larger than its mean.

Here are some examples of correlations for various variable plotted along the x- and y-axes:

![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Correlation_examples2.svg/1024px-Correlation_examples2.svg.png)

## Testing correlation



Today, let's take a look at a different version of the same dataset from last class. Now
we have two continuous variables: the city fuel efficency and the highway fuel efficency.


{% highlight r %}
cars
{% endhighlight %}



{% highlight text %}
## # A tibble: 234 x 2
##     city highway
##    <int>   <int>
##  1    13      18
##  2    13      16
##  3    12      18
##  4    21      29
##  5    16      23
##  6    15      18
##  7    22      29
##  8    14      17
##  9    19      27
## 10    17      25
## # … with 224 more rows
{% endhighlight %}

To run an hypothesis test on the correlation between these two variables, we will
use a **tmodels** function with a similar format to the other tests that we have
used. The test is called "Pearson's product-moment correlation":


{% highlight r %}
tmod_pearson_correlation_test(highway ~ city, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Pearson's product-moment correlation test
## 
## 	H0: True correlation is zero
## 	HA: True correlation is non-zero
## 
## 	Test statistic: t(232) = 49.585
## 	P-value: < 2.2e-16
## 
## 	Parameter: (Pearson) correlation coefficient
## 	Point estimate: 0.95592
## 	Confidence interval: [0.94331, 0.96577]
{% endhighlight %}

The p-value is very low (less than 0.000000000000022%) and the sample correlation
of 0.95592 is quite close to 1. We should not be surprised that these two variables
are highly related. Notice that in just this one case, it doesn't matter which variable
is treated as the response and which one is the effect (the output is exactly the same):


{% highlight r %}
tmod_pearson_correlation_test(city ~ highway, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Pearson's product-moment correlation test
## 
## 	H0: True correlation is zero
## 	HA: True correlation is non-zero
## 
## 	Test statistic: t(232) = 49.585
## 	P-value: < 2.2e-16
## 
## 	Parameter: (Pearson) correlation coefficient
## 	Point estimate: 0.95592
## 	Confidence interval: [0.94331, 0.96577]
{% endhighlight %}

There are two alternatives to Pearson's correlation test available in **tmodels**.
Both are similar to the Mann-Whitney test in that they are more robust to outliers
and bad data points but more likely to incorrectly accept the null-hypothesis when
it is not true. The first is "Kendall's rank correlation tau test":


{% highlight r %}
tmod_kendall_correlation_test(city ~ highway, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Kendall's rank correlation tau test
## 
## 	H0: True tau is zero
## 	HA: True tau is non-zero
## 
## 	Test statistic: z = 18.399
## 	P-value: < 2.2e-16
## 
## 	Parameter: Kendall's rank correlation (tau)
## 	Point estimate: 0.8628
{% endhighlight %}

And the other is "Spearman's rank correlation rho test":


{% highlight r %}
tmod_spearman_correlation_test(city ~ highway, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Spearman's rank correlation rho test
## 
## 	H0: True rho is zero
## 	HA: True rho is non-zero
## 
## 	Test statistic: S = 97781
## 	P-value: < 2.2e-16
## 
## 	Parameter: Spearman's correlation (rho)
## 	Point estimate: 0.95421
{% endhighlight %}

I will be completly honest that I do not have a good sense of when you should use
Kendall's test or Spearman's test. Just be familiar with each of them as they are
both commonly used across the sciences and social sciences.

## Testing multiple means



As a totally different direction, let's return briefly to testing the means of a random
variable by groups. Take another look at a version of the cars dataset; now it includes
information about several different manufacturers:


{% highlight r %}
cars
{% endhighlight %}



{% highlight text %}
## # A tibble: 234 x 2
##     city manufacturer
##    <int> <chr>       
##  1    13 dodge       
##  2    13 ford        
##  3    12 land rover  
##  4    21 volkswagen  
##  5    16 dodge       
##  6    15 toyota      
##  7    22 volkswagen  
##  8    14 nissan      
##  9    19 nissan      
## 10    17 audi        
## # … with 224 more rows
{% endhighlight %}

The `tmod_mean_by_group` function shows us the means of the fuel efficency for each manufacturer:


{% highlight r %}
tmod_mean_by_group(city ~ manufacturer, data=cars)
{% endhighlight %}



{% highlight text %}
##       Mean of audi  Mean of chevrolet      Mean of dodge 
##           17.61111           15.00000           13.13514 
##       Mean of ford      Mean of honda    Mean of hyundai 
##           14.00000           24.44444           18.64286 
##       Mean of jeep Mean of land rover    Mean of lincoln 
##           13.50000           11.50000           11.33333 
##    Mean of mercury     Mean of nissan    Mean of pontiac 
##           13.25000           18.07692           17.00000 
##     Mean of subaru     Mean of toyota Mean of volkswagen 
##           19.28571           18.52941           20.92593
{% endhighlight %}

What if we want to run an hypothesis test with:

- H0: The means are the same for each group.
- HA: Some of the means are different between each group.

Running the T-test will not work (if you go back to the last notes, you'll see that it cannot
be extended easily to more than two groups):


{% highlight r %}
tmod_t_test(city ~ manufacturer, data=cars)
{% endhighlight %}



{% highlight text %}
## Error in tmod_t_test(city ~ manufacturer, data = cars): The predictor variable has 15 unique values. This test requires exactly 2.
{% endhighlight %}

The "One-way Analysis of Variance (ANOVA)" test is the multiple group extension of the
T-Test:


{% highlight r %}
tmod_one_way_anova_test(city ~ manufacturer, data=cars)
{% endhighlight %}



{% highlight text %}
## 
## One-way Analysis of Variance (ANOVA)
## 
## 	H0: True means are the same in each group.
## 	HA: True means are the same in each group.
## 
## 	Test statistic: F(14, 219) = 19.613
## 	P-value: < 2.2e-16
{% endhighlight %}

The Mann-Whitney test also has a multi-group extension called the "Kruskal-Wallis rank sum test":


{% highlight r %}
tmod_kruskal_wallis_test(city ~ manufacturer, data=cars)
{% endhighlight %}



{% highlight text %}
## 
## Kruskal-Wallis rank sum test
## 
## 	H0: Location parameters are the same in each group.
## 	HA: Location parameters are not the same in each group.
## 
## 	Test statistic: chi-squared(14) = 142.69
## 	P-value: < 2.2e-16
{% endhighlight %}

Notice that these tests do not give any point estimates; they just tell us whether
there seem to be any differences in means across the groups.



