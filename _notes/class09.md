---
title: "Class 09: Multivariate Analysis with Regression"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Understand the role of nuisance variables in hypothesis testing and why we need to control for them.
- Apply the linear regression function in the **tmodels** package to test for relationships between explanatory and response variables while controlling for nuisance variables.
- Identify and use the output of the function `tmod_tmod_linear_regression` depending on the form of the explanatory variable.

## Controlling for Nuisance Variables

All of the hypothesis tests that we have so far studied have exactly
two variables: an explanatory variable (IV) and the response variable (DV).
Our tests have null hypotheses that indicate, an various appropriate ways,
that there is no relationship between the two variables. We want to see if
there is evidence to support having an actual effect between the two variables.

Today we will extend this setup to include any number of other variables that
we believe may effect the response variable and that we want to *factor out*
or *control for* in the analysis. For example, returning very briefly to the
pea plants and colored light experiment. We assumed that the plants were all
identical; what if instead some where snow peas (*saccharatum*) and others
were snap peas (*macrocarpon*). One way to deal with this is to include the
variety of the pea plant as a third variable into the model. The variety variable
as used here is what we call a **nuisance variable**.



Let's actually apply this technique to the cars dataset. Here I have selected
a larger set of variables to work with:


{% highlight r %}
cars
{% endhighlight %}



{% highlight text %}
## # A tibble: 37 x 5
##    manufacturer class   engine_size  city highway
##    <chr>        <chr>         <dbl> <int>   <int>
##  1 audi         compact         2      20      28
##  2 audi         compact         3.1    17      25
##  3 chevrolet    suv             5.3    14      20
##  4 chevrolet    suv             5.7    11      15
##  5 audi         compact         3.1    18      27
##  6 chevrolet    suv             5.3    14      19
##  7 chevrolet    suv             5.3    11      14
##  8 chevrolet    suv             5.3    11      15
##  9 chevrolet    suv             5.3    14      20
## 10 audi         compact         1.8    21      29
## # … with 27 more rows
{% endhighlight %}

If we wanted to investigate the relationship between fuel efficency and
engine size, a reasonable model would be the Spearman rank correlation Test:


{% highlight r %}
tmod_spearman_correlation_test(city ~ engine_size, data = cars)
{% endhighlight %}



{% highlight text %}
## Warning in cor.test.default(x = c(2, 3.1, 5.3, 5.7, 3.1, 5.3, 5.3, 5.3, :
## Cannot compute exact p-value with ties
{% endhighlight %}



{% highlight text %}
## 
## Spearman's rank correlation rho test
## 
## 	H0: True rho is zero
## 	HA: True rho is non-zero
## 
## 	Test statistic: S = 14702
## 	P-value: 1.414e-07
## 
## 	Parameter: Spearman's correlation (rho)
## 	Point estimate: -0.74272
{% endhighlight %}

As perhaps expected, the correlation is strongly significant and negative.
Cars with larger engines are not as efficent. What if, additionally though,
we wanted to *control for* the class of the car? That is, do we think that
the size of the engine matters if we are comparing two trucks to one another?
To do this, we run a "linear regression model" using the function
`tmod_linear_regression`. The syntax is very similar to the other models, but
on the right-hand side of the `~` you add (literally, with the plus sign) the
nusiance variables you want to account for:


{% highlight r %}
tmod_linear_regression(city ~ engine_size + class, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Linear regression; T-Test
## 
## 	H0: Change in conditional mean is zero
## 	HA: Change in conditional mean is non-zero
## 
## 	Test statistic: t = -2.849
## 	P-value: 0.007607
## 
## 	Parameter: change in city for unit change in engine_size
## 	            controlling for -- class
## 	Point estimate: -1.4908
## 	Confidence interval: [-2.55660, -0.42491]
{% endhighlight %}

And we see here that the result is still significant. The point estimate
indicates that, after accounting for the class of the car, each additional
litre in engine size decreases the city fuel efficency by about 1.49 miles
per gallon.

We can use the exact same syntax with an explanatory categorical input while
controlling for nuisance variables.


{% highlight r %}
tmod_linear_regression(city ~ manufacturer + class, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Linear regression; T-Test
## 
## 	H0: Difference in conditional mean is zero
## 	HA: Difference in conditional mean is non-zero
## 
## 	Test statistic: t = 2.2999
## 	P-value: 0.02813
## 
## 	Parameter: mean(city|chevrolet) - mean(city|audi)
## 	           after controlling for -- class
## 	Point estimate: 2.8
## 	Confidence interval: [0.3201, 5.2799]
{% endhighlight %}

The output looks a bit different, but the underlying statistical model is
exactly the same.

Finally, we can also test whether fuel efficency is effect by the class of
the car after accounting for the manufacturer.


{% highlight r %}
tmod_linear_regression(city ~ class + manufacturer, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Linear regression; F-Test
## 
## 	H0: Difference in conditional mean is zero
## 	HA: Difference in true means is non-zero
## 
## 	Test statistic: F(1, 3) = 15.753
## 	P-value: 1.812e-06
{% endhighlight %}

The input is exactly the same, but the output now looks different. Because
there are more than two classes of cars we need a different test and cannot
easily provide point estimates. This is the equivalent of the one-way ANOVA,
but now in a regression framework where we can control for the nuisance variables.

## Assumptions of linear regression models

The linear regression model assumes the two standard assumptions we have across
all of our models: uniform sampling and independent response values. There are
additionally a host of other assumptions that all well beyond the scope of this
course. Just know that as long as you (approximately) have the two basic
assumptions, you are usually safe using linear regression for testing the
relationship between a continuous response and any explanatory variable.






