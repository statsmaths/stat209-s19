---
title: "Class 10: Logistic Regression and One-sample tests"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Apply logistic regression to model the probability of an event as the response variable, both with and without nusiance variables.
- Understand the log-odds of an event and how it is used in logistic regression.
- Apply one-sample tests for a mean.

### Modeling probabilities


{% highlight r %}
data(mpg, package = "ggplot2")
cars <- select(mpg, city = cty, highway = hwy, class)

set.seed(1)
cars$green_award <- NULL
cars$green_award <- as.character((cars$city > 20) & (runif(nrow(cars)) > 0.6))
{% endhighlight %}


Looking back through your notes, you'll notice that we have not yet worked
with an inference problem where the response (DV) variable is categorical and
the effect (IV) variable is continuous. To do this, we will make use of logistic
regression. 

If you are only concerned with inference, logistic regression is very straightforward
and virtually unchanged from linear regression. Let's take the cars dataset one last
time and look at whether a car has been awarded a "Green Energy Award" as a function
of its fuel efficency in the city:


{% highlight r %}
tmod_logistic_regression(green_award ~ city, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Logistic regression; Z-Test
## 
## 	H0: Change in conditional log odds is zero
## 	HA: Change in conditional log odds is non-zero
## 
## 	Test statistic: Z = 5.2015
## 	P-value: 1.976e-07
## 
## 	Parameter: change in LO(green_award=TRUE) for unit change in city
## 	           controlling for -- 
## 	Point estimate: 0.38922
## 	Confidence interval: [0.24256, 0.53588]
{% endhighlight %}

The p-value here tells us the probability of observing an effect at least as strong
as shown in the data if there is no change in the probability of a green award as a
function of the city fuel efficency.

We can also extend this to a categorical variable (and it becomes an F-test):


{% highlight r %}
tmod_logistic_regression(green_award ~ class, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Logistic regression; F-Test
## 
## 	H0: Difference in conditional log odds is zero
## 	HA: Difference in true log odds is non-zero
## 
## 	Test statistic: Rao = 33.589
## 	P-value: 8.074e-06
{% endhighlight %}

And we can include any number of nusiance variables into the model:


{% highlight r %}
tmod_logistic_regression(green_award ~ city + highway + class, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Logistic regression; Z-Test
## 
## 	H0: Change in conditional log odds is zero
## 	HA: Change in conditional log odds is non-zero
## 
## 	Test statistic: Z = 2.2774
## 	P-value: 0.02276
## 
## 	Parameter: change in LO(green_award=TRUE) for unit change in city
## 	           controlling for -- highway; class
## 	Point estimate: 0.52502
## 	Confidence interval: [0.073178, 0.976860]
{% endhighlight %}

So far this is fairly straightforward, and most of the time this is the most
import part (inference) of logistic regression in applications.

## Logistic regression

How does logistic regression actually work and what are the point estimates
telling us? Let's look at these two questions now.

Assume for a moment that we have a variable p that tells us the probability of
observing a particular event. The **odds** of this event is the ratio of p divided
by 1-p (the probability of not observing p):

$$ \text{odds} = \frac{p}{1-p} $$

This quantity is often used in gambling contexts. Notice that an odds of 1
indicates that the probability is equal to 0.5; it is just as likely to observe
the event as it is to not observe the event.

The **log odds** is simply the (natural) logarithm of the odds:

$$\text{log odds} = \log \left( \frac{p}{1-p} \right) $$

Logistic regression tries to model the logs odds of an event. Specifically, it
tries to build a model that look like:

$$\text{log odds} = a + b \cdot X $$

For some variable X. Why model the log odds and not the probability itself?
The probability can only be between 0 and 1, whereas the log odds can range
over and real number. The right-hand side above could take on any value (depending
on the inputs X), and so it is much more mathematically sound to model the
log odds rather than the probability directly. The formulation given here also
a high-degree of symmetry about what class we pick as the event (specifically, if
we flip 0 and 1 in the input, the values of the slopes and intercepts simply
switch signs). 

Looking again at the logistic regression of the green award as a function of 
the city variable, how do we interpret the parameter value?


{% highlight r %}
tmod_logistic_regression(green_award ~ city, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## Logistic regression; Z-Test
## 
## 	H0: Change in conditional log odds is zero
## 	HA: Change in conditional log odds is non-zero
## 
## 	Test statistic: Z = 5.2015
## 	P-value: 1.976e-07
## 
## 	Parameter: change in LO(green_award=TRUE) for unit change in city
## 	           controlling for -- 
## 	Point estimate: 0.38922
## 	Confidence interval: [0.24256, 0.53588]
{% endhighlight %}

We say that the logs odds increases by 0.3892 for every unit of increase in the city
miles per gallon fuel efficency of the car. Is this very satisfying? Probably not,
because there is not way to directly map this statement into things about probabilites.
It is just what we are stuck with and the best that we can do.

## One-sample tests


{% highlight r %}
data(mpg, package = "ggplot2")
cars <- select(mpg, city = cty, highway = hwy)
{% endhighlight %}

There is one final type of test that we have not seen yet that is fairly common in
applied statistics. It is actually one of the easiest tests to understand and is often
the first type of test introduced in a statistics course. I choose to do it last because
while the theoretical concepts are easy to understand I find it harder to explain why the
test might be useful.

Specifically, consider the hypothesis test for a variable x defined by:

$$ H_0: \text{The mean of x is equal to } \mu.  $$

$$ H_A: \text{The mean of x is not equal to } \mu.  $$

For some value of the parameter mu. This at first looks a lot like the tests we have already
done, but it involves testing the exact value of mean variable's mean rather than looking
at the difference between the means of two variables. This is called a one-sample test.

The two most common one-sample tests are: the one-sample t-test (parametric) and the
Wilcoxon test (non-parametric). To run this with the **tmodels** package, use the
function `tmod_t_test_one` and just put the number `1` on the right-hand-side. There
are no independent variables in this test; the `1` serves as just a place-holder.


{% highlight r %}
tmod_t_test_one(city ~ 1, data = cars)
{% endhighlight %}



{% highlight text %}
## 
## One Sample t-test
## 
## 	H0: Mean is equal to 0.000000
## 	HA: Mean is not equal to 0.000000
## 
## 	Test statistic: t(233) = 60.596
## 	P-value: < 2.2e-16
## 
## 	Parameter: Average value
## 	Point estimate: 16.859
## 	Confidence interval: [16.311, 17.407]
{% endhighlight %}

The point estimate gives the mean of the city variable and the confidence interval
gives a range of guesses for the mean (NB: In my experience, the confidence intervals
are actually more common and useful with one-sample tests than the p-value). We can
change the value of mu that defines the null hypothesis by inputing the parameter `h0`
in R:


{% highlight r %}
tmod_t_test_one(city ~ 1, data = cars, h0 = 16)
{% endhighlight %}



{% highlight text %}
## 
## One Sample t-test
## 
## 	H0: Mean is equal to 16.000000
## 	HA: Mean is not equal to 16.000000
## 
## 	Test statistic: t(233) = 3.0874
## 	P-value: 0.002264
## 
## 	Parameter: Average value
## 	Point estimate: 16.859
## 	Confidence interval: [16.311, 17.407]
{% endhighlight %}

The non-parametric test works similarly:


{% highlight r %}
tmod_wilcoxon_test(city ~ 1, data = cars, h0 = 16)
{% endhighlight %}



{% highlight text %}
## 
## Wilcoxon One-Sample Test
## 
## 	H0: Location shift is equal to 16.000000
## 	HA: Location shift is not equal to 16.000000
## 
## 	Test statistic: V = 13894
## 	P-value: 0.01205
{% endhighlight %}


