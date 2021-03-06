---
title: "Class 11: Inference Review"
author: "Taylor Arnold"
output: html_document
---



<style>
#dtable {
  font-family: "Trebuchet MS", Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

#dtable td, #dtable th {
  border: 1px solid #ddd;
  padding: 8px;
}

#dtable tr:first-child{background-color: #f2f2f2;}

#dtable tr:hover {background-color: #ddd;}

#dtable th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: center;
  background-color: #4CAF50;
  color: white;
}
</style>

### Learning Objectives

- Understand the dangers of multiple hypothesis testing
- Apply `p.correct` to control the family-wise error rate of several tests
- Understand how to *knit* an Rmarkdown file to an HTML document
- Review for Exam II

### Multiple hypothesis testing

Multiple hypothesis testing is a common issue when working with hypothesis
tests in real research. I think this comic does a better job of explaining
the problem than I could with just words:

![](https://imgs.xkcd.com/comics/significant.png)

Multiple hypothesis testing, then, is the problem that comes from testing a lot
of statistical hypothesis tests and cherry-picking the one that is significant.
If we do this, it is no longer surprising that one of the many things we selected
for is significant at the 0.05 or even 0.005 level. It is releated, though distinct,
from the problem of publication bias.

Pre-registering results and having outlets for null-results do help with the problem
of multiple hypothesis testing. However, unlike publication bias, however, there is
also a mathematical way of fixing the problem. We can apply a p-value adjustment that
controls for cherry-picking the most interesting results; here is the R code applied
to the summary of 5 particular hypothesis tests (it implements something called the
Holm-Bonferroni correction):


{% highlight r %}
p.adjust(c(0.001, 0.01, 0.03, 0.04, 0.54))
{% endhighlight %}



{% highlight text %}
## [1] 0.005 0.040 0.090 0.090 0.540
{% endhighlight %}

You will see that only the smallest two p-values are still significant at the 0.05
level; the third and fourth were significant but no longer are.

Generally, if you are running an experiment that runs a number of hypothesis tests
trying to see what effects a particular outcome variable, you should use a multiple
hypothesis correction to account for this.

### kniting Rmarkdown

For the remainder of the exams, you will have to compile your R code to an
HTML file and print out the results. I will show you in class how this works
by walking us through how to compile the lab10.Rmd file you had from the
last class. If you are looking back through the notes this is the general
approach:

- Finish all of the code in the lab; make sure that you do not have any
commands that install packages remaining!
- Hit the "Knit" button (it includes a cute ball of yarn as a pun, making it easy to find)
- After a few seconds it should produce an HTML file saved wherever your
RMarkdown file is saved.
- If it is taking a long time, or there is any error, look at the console
to see where the bug is.

Note that the RMarkdown knit process starts completely from scratch. Common
errors include removing code that reads in datasets of libraries; make sure
that you keep any of these at the top of the file.

### Format of exam

The second exam will have two parts, a take-home portion (1/2 of the total
grade) and an in-class portion (1/2 of the grade). The take home exam will be
similar to Lab11, but will pull more generally from all of the material from
the past two weeks. I will make this available today; you must complete, knit,
print the output, and bring in this part on Thursday.

The in-class portion will be similar to the first exam, consisting of several
questions for you to do without any notes or need of a computer. I will provide
a list of all of the functions in **tmodels** on the first page, but no other
information about how to use the functions. There are four kinds of questions
I may ask you to answer:

- write the R code to run a statistical model
- write the R code to control for multiple hypothesis testing
- interpret the output from a statistical hypothesis test in R:
    - find the p-value and determine if it is significant
    - understand what the point estimate and/or confidence interval are measuring
    - interpret the sign (negative/positive) of the point estimate
- fill-in the correct name of a statistical test given its properties or applications
    - keep in mind whether it is a univariate test, bivariate test, or test with confounding variables
    - pay attention to the data type of the independent and dependent variables
    - note where and when the non-parameteric tests are preferred to the parameteric tests
- read the abstract to a scientific paper that uses hypothesis testing and understand how to interpret the results

I will give you the entire class period to work on the exam, however I think
you will only need 20-30 minutes at most if you're prepared. My guess is that
the most common grade will be a perfect score but at least one person will
fail (though I'd love to be wrong about the last prediction). This means that
it is very important to make sure that you study!

### Inference tests

In general, how do you pick which model to use? First determine if one of
the follow hold:

- If you have confounding variables use linear regression if you have
a continuous output variable and logistic regression for a categorical
output.
- If you have only a single variable, use one of the two univariate tests.
Choose between them based on whether you want a parametric or non-parametric
test (see below).

Otherwise, you'll need a bivariate test, which requires some additional
considerations:

1. Determine the type of the input and output variables. Find the appropriate
tests on the table below.
2. If you need a point estimate, have small sample sizes, or can be fairly
certain there are no outliers or bad data points, select the relevant
parametric test. Otherwise use the more robust non-parametric test. You may
always use both if you want to first show that your result is robust but then
use a parametric test to obtain a point estimate.
3. This should generally limit you to just one applicable test. If not:
  - For non-parametric correlations: Spearman's and Kendall's are both equally appropriate. Just pick one.
  - For contingency tables: if it is unclear from assumptions that we mentioned in the first unit which test to use, generally prefer the Z-test over the others and the Chi-squared over Fisher's test.

I thought it might be useful to have a single summary table with all of the statistical
tests in one place. Here they all are; make sure that if you are unclear on any of the
assumptions or difference between these test that you ask **today**.

**Univariate Test** (one variable; just response)

<table id="dtable">
<tr>
  <td>Input Type (IV)</td>
  <td>Output Type (DV)</td>
  <td>P/NP?</td>
  <td>Name</td>
  <td>Function</td>
</tr>
<tr>
  <td>&mdash;</td>
  <td>numeric</td>
  <td>P</td>
  <td>One Sample T-Test</td>
  <td>`tmod_t_test_one`</td>
</tr>
<tr>
  <td>&mdash;</td>
  <td>numeric</td>
  <td>NP</td>
  <td>Wilcoxon Test</td>
  <td>`tmod_wilcoxon_test`</td>
</tr>

<div style="padding:25px"></div>

**Bivariate Test** (two variables; input and output)

<table id="dtable">
<tr>
  <td>Input Type (IV)</td>
  <td>Output Type (DV)</td>
  <td>N/NP?</td>
  <td>Name</td>
  <td>Function</td>
</tr>
<tr>
  <td>categorical (2)</td>
  <td>categorical (2)</td>
  <td>P</td>
  <td>Z-Test for Equality of Proportions</td>
  <td>`tmod_z_test_prop`</td>
</tr>
<tr>
  <td>categorical (2+)</td>
  <td>categorical (2+)</td>
  <td>P</td>
  <td>Fisher's Exact Test</td>
  <td>`tmod_fisher_test`</td>
</tr>
<tr>
  <td>categorical (2+)</td>
  <td>categorical (2+)</td>
  <td>P</td>
  <td>Pearson's Chi-squared</td>
  <td>`tmod_chi_squared_test`</td>
</tr>
<tr>
  <td>categorical (2)</td>
  <td>numeric</td>
  <td>P</td>
  <td>Two-Sample T-Test</td>
  <td>`tmod_t_test`</td>
</tr>
<tr>
  <td>categorical (2)</td>
  <td>numeric</td>
  <td>NP</td>
  <td>Mann-Whitney rank sum</td>
  <td>`tmod_mann_whitney_test`</td>
</tr>
<tr>
  <td>categorical (2+)</td>
  <td>numeric</td>
  <td>P</td>
  <td>One-way ANOVA</td>
  <td>`tmod_one_way_anova_test`</td>
</tr>
<tr>
  <td>categorical (2+)</td>
  <td>numeric</td>
  <td>NP</td>
  <td>Kruskal-Wallis rank sum</td>
  <td>`tmod_kruskal_wallis_test`</td>
</tr>
<tr>
  <td>numeric</td>
  <td>numeric</td>
  <td>P</td>
  <td>Pearson's product-moment correlation</td>
  <td>`tmod_pearson_correlation_test`</td>
</tr>
<tr>
  <td>numeric</td>
  <td>numeric</td>
  <td>NP</td>
  <td>Spearman's rank correlation</td>
  <td>`tmod_spearman_correlation_test`</td>
</tr>
<tr>
  <td>numeric</td>
  <td>numeric</td>
  <td>NP</td>
  <td>Kendall's rank correlation</td>
  <td>`tmod_kendall_correlation_test`</td>
</tr>
</table>

<div style="padding:25px"></div>

**Multivariate Test** (2+ variables; IV, DV, and possible confounding variables)

<table id="dtable">
<tr>
  <td>Input Type (IV)</td>
  <td>Output Type (DV)</td>
  <td>N/NP?</td>
  <td>Name</td>
  <td>Function</td>
</tr>
<tr>
  <td>categorical</td>
  <td>numeric</td>
  <td>P</td>
  <td>Linear Regression</td>
  <td>`tmod_linear_regression`</td>
</tr>
<tr>
  <td>any</td>
  <td>categorical (2)</td>
  <td>P</td>
  <td>Logistic Regression</td>
  <td>`tmod_logistic_regression`</td>
</tr>
</table>






