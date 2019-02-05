---
title: "Class 06: Publication Bias and Review"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Understand the challenges of publication bias in scientific research and
resulting replication crisis.
- Identify potential approaches for addressing the challenges of publication
bias.
- Review for Exam I

## Publication bias

Throughout this semester, we will discuss a number of challenges and problems
that arise in making real-world decisions as a result of statistical evidence.
The first challenge I want to discuss is called **publication bias**. It
arises because researchers tend to only publicize interesting results. If an
experiment is run and the treatment variable seems to have little to no effect
on the dependent variable, it is unlikely for the research to every be
discussed or published.

Why is publication such a problem? Assume that you see an article that says
that pea plants survive longer when exposed only to red LED lights. The
experimental design seems solid and Z-test for the difference in proportions
has a p-value of 0.05; the chances that these results are just due to random
chance is fairly low. __However__, what if 19 other scientists ran the same
experiment and found no evidence of a statistically significant difference
between the light color and pea plant survival? Because of publication bias,
the results of the other 19 groups may never be known to you, the pea farmer
looking to invest in artificial lights for your growing pea farm. This is a
problem, because if we look at all of the data, over 20 studies we would
expect there to be at least one with a p-value less than 0.05 even if there
is no true effect in the use of red LED lights.

While it may be unlikely that there were 19 unknown studies, conflated with
other challenges (which we will discuss, including p-hacking and multiple
hypothesis testing), publication bias has the potential to make most published
scientific claims dubious at best. There are, fortunately, several approaches
for addressing this challenge:

- pre-registering results
- outlets for null results
- support for replication studies
- better informing journalists and public about challenges of
over-generalizing and over-hyping a single study

More on the specifics of these in the second unit of the course.

## Review

### Inferential Statistics

The general method of doing inferential statistics is:

1. Start with a research question
2. Turn research question into a Null Hypothesis and Alternative Hypothesis
3. Collect a lot of data; store it in a tabular format following our naming conventions
4. Run an appropriate hypothesis test based on the data type and model assumptions; record the p-value
5. Interpret the results, keeping in mind whether this is an observational or experimental study

Step 4 can be done either directly using the R code from the first few classes
or using the functions in the **tmodels** package. The steps in the direct
 approach are:

1. Describe a test statistic in terms of the observed data.
2. Simulate the *sampling distribution* to find the distribution of the test statistic under the null hypothesis.
3. Compare the test statistic on your observed data with the sampling distribution.

The direct method is important to understand because it explains *how* the
hypothesis test works and exactly *what* the p-value is measuring. However,
you in practice shouldn't use this approach. Therefore, I am never going to
ask you to know how to run the code for this direct method (the concepts above
though are valid topics for exam questions).

The approach with the **tmodels** package requires you to simply read in the
data and run the relevant hypothesis test. Here is the complete code for
doing this. Start with loading all of the packages you might need (some of
these are not 100% necessary at the moment, but I like to get in the habit of
loading them):


{% highlight r %}
library(readr)
library(readxl)
library(dplyr)
library(ggplot2)
library(tmodels)
{% endhighlight %}

Then, read in your dataset and run the functions from **tmodels**:


{% highlight r %}
hlas <- read_xlsx("lab05-table3.xlsx")

tmod_contingency(las ~ height, data=hlas)

tmod_fisher_test(las ~ height, data=hlas)
tmod_chi_squared_test(las ~ height, data=hlas)
tmod_z_test_prop(las ~ height, data=hlas)
{% endhighlight %}

Here I have just given you the code for all three tests as well as a function
for printing out the contingency table. Here the columns in the dataset are
called `las` and `height`. The variable `height` in the input/explanatory
variable and `las` is the output/response variable. I thought it would be
easier to include all of the tests together in one block.

Here is what the output from the above code looks like:


{% highlight text %}
##          Response
## Predictor absent present
##     short     18       1
##     tall      17       7
{% endhighlight %}



{% highlight text %}
## 
## Fisher's Exact Test for Count Data
## 
## 	H0: Group variable and response categories are independent
## 	HA: Group variable and response categories are dependent
## 
## 	P-value: 0.05928
{% endhighlight %}



{% highlight text %}
## 
## Pearson's Chi-squared test with Yates' continuity correction
## 
## 	H0: Group variable and response categories are independent
## 	HA: Group variable and response categories are dependent
## 
## 	Test statistic: chi-squared(1) = 2.5785
## 	P-value: 0.1083
{% endhighlight %}



{% highlight text %}
## 
## Z-Test for Equality of Proportions (2 groups)
## 
## 	H0: true probability of effect is the same between groups
## 	HA: true probability of effect is difference between groups
## 
## 	Test statistic: Z = -2.2554
## 	P-value: 0.02411
## 
## 	Parameter: Pr(present|short) - Pr(present|tall)
## 	Point estimate: -0.23904
## 	Confidence interval: [-0.446760, -0.031311]
{% endhighlight %}

### Model Assumptions

There are two important model assumptions that must hold for any of the hypothesis
tests above to be valid:

1. the data were sampled uniformly from the population of interest
2. the response variable values are unrelated (i.e., independent) to one another

Each of the three tests has the same null and alternative hypothesis (that there is
no effect on the output groups from the input groupings). The primary difference is only
in terms of what values are assumed to be know ahead of the experiment:

- **Fisher's exact test**: row and column sums are known ahead of the data collections (double-conditioned; very rare)
- **Z-test for proportions**: row sums are known ahead of time, but column sums are not (single-conditioned; common in experimental studies)
- **chi-squared text**: neither row nor column sums are known ahead of the data collection (un-conditioned; common in observational studies)

There are also a difference in the number of samples typically required before people
fell comfortable using each test:

- **Fisher's test**: valid for any sample size
- **Z-test for proportions**: each row should have at least 5 samples
- **chi-squared test**: each cell should have at least 5 samples

The exact cutoffs sample sizes for the latter two tests vary somewhat from source to source.

### Other concepts and things to know

Make sure that you are familiar with the following concepts:

- experimental vs. observational studies
- unit of observation
- publication bias
- naming conventions for variables

Further, you should be able to do the following things from memory in R
(there is no other code that I would ask you to write for the in-class exam):

- how to load an R package
- how to use the function `tmod_z_test_prop` to run an hypothesis test




