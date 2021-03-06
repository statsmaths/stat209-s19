---
title: "Class 07: Inference for the Mean"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Identify model assumptions and setup similarities that extend our inference techniques on 2x2 tables to other domains.
- Classify variables as either numeric or categorical.
- Apply the formula for a (sample) mean to a dataset and understand what it measuring.
- Apply the (two-sample) T-test and Mann-Whitney rank sum test to data in R.

## Next Unit

Over the next two weeks we will extend our statistical inference framework
to include many more set-ups beyond two-by-two contingency tables. Much will,
however, remain unchanged. For example, these steps all remain the same:

1. Start with a research question
2. Turn research question into a Null Hypothesis and Alternative Hypothesis
3. Collect a lot of data; store it in a tabular format following our naming conventions
4. Run an appropriate hypothesis test based on the data type and model assumptions; record the p-value
5. Interpret the results, keeping in mind whether this is an observational or experimental study

Further, all of tests we will look at require the two following assumptions
in order to be valid:

1. the data were sampled uniformly from the population of interest
2. the response variable **values** are unrelated (i.e., independent) to one another

The differences between observational and experimental studies, and their impact
on whether we can make causal claims, also carry over directly to these new tests.
Of course, the tests will additionally have other requirements specific to
each application. Finally, all of the statistical tests we will look at has
a null and alternative hypothesis that can be approximately described by:

- H0: there is no relationship between the explanatory (IV) and response (DV) variables
- HA: there is a relationship between the explanatory (IV) and response (DV) variables

The specific nature of the relationship will depend on the specific test.

## Variable types

A particular variable can usually be classified as belonging to one of two
broad categories:

- **numeric variables**: represented as a single number
- **categorical variables**: represented as observations from a closed set of options

There are other variable types, as well as some grey areas where a variable may
be somewhere in-between fully numeric and fully categorical. In EDA, these grey
areas will be quite important; in statistical inference it is usually clear whether
a variable in numeric or categorical.

We have already worked with the case where both of our variables are categorical.
The extensions we will see these next two weeks focus on when one or both of the
variables are instead numeric. We will also look at the case where both variables
are categorical but one or both have more than two unique values.

## The mean of a variable

We need to define two numeric quantities in order to talk about hypothesis testing
with numeric variables. You are likely already familiar with the mean, or average,
of a numeric variable. Just for completeness, the average value of a set of
observations is computing by adding all of the observations together and dividing
by the number of values that are observed (which I will write as the variable n):

$$ \text{mean} (x) = \bar{x} = \frac{x_1 + x_2 + \cdots + x_n}{n} $$

Note that we use a bar to represent the mean of a variable.

## Testing differences in the mean



The hypothesis testing framework that we want to look at today is the case
where we have data split into two groups (the explanatory variable) and
observe a continuous response. Our question is to figure out whether we believe
that the average value of the response is effected by the grouping variable if
we were to collect the same data over an entire population.

Let's look at a specific example. Here I have a dataset giving the fuel efficency
of automobiles from two manufacturers, Audi and Chevrolet, in the stop and go
city traffic. Fuel efficency is given in miles per gallon.


{% highlight r %}
cars
{% endhighlight %}



{% highlight text %}
## # A tibble: 37 x 2
##    manufacturer  city
##    <chr>        <int>
##  1 audi            20
##  2 audi            17
##  3 chevrolet       14
##  4 chevrolet       11
##  5 audi            18
##  6 chevrolet       14
##  7 chevrolet       11
##  8 chevrolet       11
##  9 chevrolet       14
## 10 audi            21
## # … with 27 more rows
{% endhighlight %}

What we want to know is whether there is a statistically significant difference
between the fuel effiency of Audi cars compared to Chevrolet cars. We can find
the *sample means* (the average value for this particular dataset) using:


{% highlight r %}
tmod_mean_by_group(city ~ manufacturer, data = cars)
{% endhighlight %}



{% highlight text %}
##      Mean of audi Mean of chevrolet 
##          17.61111          15.00000
{% endhighlight %}

So, in this sample Audi cars have better fuel efficency. We need to perform statistical
inference, however, to see whether this difference is due to chance or indicates strong
evidence that there is a real difference between the manufacturers. In order to do this
we run a (two-sample) T-Test. This is easy with the **tmodels** package:


{% highlight r %}
tmod_t_test(city ~ manufacturer, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Two Sample t-test
## 
## 	H0: Difference in true means is zero
## 	HA: Difference in true means is non-zero
## 
## 	Test statistic: t(31.712) = 3.1974
## 	P-value: 0.003137
## 
## 	Parameter: Difference in means (audi - chevrolet)
## 	Point estimate: 2.6111
## 	Confidence interval: [0.9471, 4.2751]
{% endhighlight %}

Looking at the output, we see that if there was no actual difference in the
means of the two groups we would only see results this extreme 0.3% of the time.
This is fairly rare, and we would typically reject the null hypothesis that
there is no difference between the groups.

Just as with the 2x2 contingency tables, there are several alternatives to the
two-sample T-test. The only one we will look at here isthe Mann-Whitney rank
sum test.


{% highlight r %}
tmod_mann_whitney_test(city ~ manufacturer, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Mann-Whitney rank sum test with continuity correction
## 
## 	H0: True location shift is equal to zero
## 	HA: True location shift is not equal to zero
## 
## 	Test statistic: W = 267.5
## 	P-value: 0.003308
{% endhighlight %}

The Mann-Whitney rank sum test is more robust to outliers and is not greatly effected by a few bad data
points (compared to the T-test, which is quite sensitive). However, it is also less *powerful*: there is a
higher chance it will fail to reject the null-hypothesis even when it is not true.

## Test Statistics, T-Test and CLT

In this class I want to focus on making you familiar with what hypothesis tests
you may need or see in future work. I want you to understanding how to run these
tests, know when they are appropriate, and be able to interpret the output from
R. We are not going to get as much into the details of *how* and *why* these
tests work they way I claim that they do.

From experience, I know that some students have trouble with the idea that some
things have to be interpreted as a black box. I'll try to explain just a few more
details here. The T-test works by assuming that we have a sample of data from one
group:

$$ x_1, x_2, \ldots, x_m $$

And another sample of data from another group

$$ z_1, z_2, \ldots, z_n $$

Then, we define the summary statistics:

$$ s_x^2 = \frac{1}{m-1} \cdot \sum_{i=1}^m (x_i - \bar{x})^2  $$

$$ s_y^2 = \frac{1}{n-1} \cdot \sum_{i=1}^n (y_i - \bar{y})^2  $$

$$ df = \frac{\left[\frac{s_x^2}{m} + \frac{s_y^2}{n}\right]^2}{\frac{\left[\frac{s_x^2}{m}\right]^2}{m-1} +\frac{\left[\frac{s_y^2}{n}\right]^2}{n-1}} $$

Finally, we define the test statistic:

$$ T = \frac{\bar{x} - \bar{y}}{\sqrt{\frac{s_x^2}{m} + \frac{s_y^2}{n}}} = \frac{\bar{x} - \bar{y}}{SE} $$

Why do we use this fancy scaling of the differences in the means? It turns out that
probability theory tells us that the sampling distribution of T (under the null hypothesis
that each group has the same underlying mean) depends **only** on the value of `df`, the
effective degress of freedom, thanks to something called the Central Limit Theorem (CLT).
This means that we can get a p-value without having to run
any simulations or needing to know the specific distributions from which the x's and y's
were drawn from. You will see that the value of T and df are given in R's output.
As you can imagine, deriving these formulas and proving that they have the
desired properties is a complex and lengthy process. The last steps require at least three
semesters of calculus and an entire semester of 300-level probability theory even to get
started.

All of the other statistical tests we have and will see in this course have a similar
derivation: there's a complex formula to define a test statistic that has a known distribution
under the null hypothesis. Proving these things requires some powerful mathematics. We will
stick, from here on out, to leaving this black box shut so that we can concentrate on the
qualitative aspects of hypothesis testing.

<p><img src="https://i5.walmartimages.com/asr/83a5d9fe-ea48-4c78-b729-70045b6712b6_1.62407b7f4beecc0c48fe0b205b93dbf9.jpeg" style="height: 300px; display: block; margin-left: auto; margin-right: auto" /></p>




