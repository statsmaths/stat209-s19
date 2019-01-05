---
title: "Class 02: Difference in Proportions"
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

#dtable tr:nth-child(even){background-color: #f2f2f2;}

#dtable tr:hover {background-color: #ddd;}

#dtable th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: #4CAF50;
  color: white;
}
</style>

### Learning Objectives

- Learn the goal of inferential statistics.
- Understands the general concepts behind statistical inference in a simple
2-by-2 contingency table.
- Distinguish between the **explanatory variable(s)** and the
**response variable** in an experiment.

### Inference versus EDA

There are several approaches to the analysis of data. One method is
called *statistical inference*, or just *inference*, and involves trying to
learn things about data that you did not collect from the data that you did
collect. This is a common approach in the sciences and social sciences and
includes applications such as:

- a pharmaceutical drug trial
- a market research test for a new product
- polling research
- psychological studies

The other approach is *exploratory data analysis* (EDA). Here, there is not
usually a fixed question of interest but rather a large dataset that we want
to explore for interesting patterns. There may be some qualitative attempt to
infer things from data that has not been seen, but that is not typically the
focus.

We will start by looking at inferential data analysis this semester (a change
from other times I have taught this course), with time to do more EDA pushed
to the final section of the course.

You should also note that there is a third form of data analysis known as
*predictive modelling*, most prominent within computer science and
engineering. Here the goal is to make predictions about individual future
observations rather than inferences about entire populations of objects or
events. We may briefly talk about this form of analysis if time permits.

### A contingency table

Let's start with a relatively simple experiment. We will take 100 newly
planted pea plants and place them under a white LED light. It will look
something like this:

<img src="https://i1.wp.com/foodiegardener.com/wp-content/uploads/2015/01/LED-bulb-grow-light-lettuce-foodie-gardener-blog-e1422601087389.jpg" style="height: 200px; display: block; margin-left: auto; margin-right: auto"/>

And then, we'll take 100 newly planted pea plants and place them under
a blue LED light:

<img src="http://i.ebayimg.com/images/i/350641586971-0-1/s-l1000.jpg" style="height: 300px; display: block; margin-left: auto; margin-right: auto"/>

In all other respects the plants will be treated the same (same seed
source, same water, same temperature, ect.). After 30 days, we measure
whether a particular plant is still alive. Our research question is
to determine whether pea plants grow better under white light compared
to just blue light.

What is the data from this experiment going to look like? One easy way
to summarize the data is through a table such as this one:

<table id="dtable">
<tr>
  <td></td>
  <td>Died (D)</td>
  <td>Survived (S)</td>
</tr>
<tr>
  <td>White light (W)</td>
  <td>40</td>
  <td>60</td>
</tr>
<tr>
  <td>Red light (B)</td>
  <td>50</td>
  <td>50</td>
</tr>
</table>



This is called a **contingency table**, specifically a 2-by-2 (or 2x2)
contingency table. Notice that all of the information about the study
is captured by these four numbers. The rows describe the explanatory
variable (also known by many other names: independent, regressor, covariate,
predictor) and the columns describe the response variable (also known by:
regressand, criterion, predicted variable, measured variable,
explained variable, experimental variable, responding variable,
outcome variable, output variable, and label).

### Summary statistics from 2x2 contingency table

The research question we have is whether the probability of survival
of the plants under the white light is different than the probability
of survival under the red light. What is the proportion of plants that
survived under the white light? Let's call this $\widehat{p}_W$ and
compute it through:

$$ \widehat{p}_W = \frac{\text{survived white light}}{\text{total white light}} = \frac{60}{60 + 40} = \frac{60}{100} = 0.6 = 60\% $$

Similarly, the probability of surviving under the red light in the
experiment was measured as:

$$ \widehat{p}_R = \frac{\text{survived red light}}{\text{total red light}} = \frac{50}{50 + 50} = \frac{50}{100} = 0.5 = 50\% $$

So, 60% of the plants with the white light survived and only 50% with
the red light survived. The *observed* difference in probabilities in
our experimental sample is, therefore, 10%.

## Inference on a 2x2 contingency table

I said earlier that the goal of inference is to *infer* something
about a larger population from the result of an experiment. So far
we have only said something about the data that we actually observered.
What we want to infer here is the effect of white versus blue LED lights
when the same parameters are applied to any set of pea plants (e.g., not
just the 200 in this particular experiment).

I used the hat notation ($\widehat{p}$) above to indicate that the
values are from a particular experiment. The values $p_W$ and $p_R$
are the "true" probabilites of survival if we repeated the experiment
with every single pea plant seed in the world. We do not know these
two values. What can we say about $D = p_W - p_R$ if we only know the
value $\widehat{D} = \widehat{p}_W - \widehat{p}_R$? This is **the**
question that much of statistical theory attempts to answer.

Let's simplify our question a bit by focusing on a sub-question. Given
the experimental evidence, how strongly do we feel that the "true" value
of $D$ is greater than 0? That is, how certain can we be that white light
provides better survival odds than red light?

### Inference on a 2x2 contingency table -- simulation

One way to address this question is to start by assuming that
the probabilites are equal and both are equal to 50%.

$$ p_W = p_R = 0.5 $$

Then, we can use **R** to simulate what would happen if this
were the case. Here is the code to select 100 values from the
options `survived` and `died`:


{% highlight r %}
white <- sample(c("survived", "died"), size=100, replace=TRUE)
white
{% endhighlight %}



{% highlight text %}
##   [1] "survived" "survived" "died"     "died"     "survived" "died"    
##   [7] "died"     "died"     "died"     "survived" "survived" "survived"
##  [13] "died"     "survived" "died"     "survived" "died"     "died"    
##  [19] "survived" "died"     "died"     "survived" "died"     "survived"
##  [25] "survived" "survived" "survived" "survived" "died"     "survived"
##  [31] "survived" "died"     "survived" "survived" "died"     "died"    
##  [37] "died"     "survived" "died"     "survived" "died"     "died"    
##  [43] "died"     "died"     "died"     "died"     "survived" "survived"
##  [49] "died"     "died"     "survived" "died"     "survived" "survived"
##  [55] "survived" "survived" "survived" "died"     "died"     "survived"
##  [61] "died"     "survived" "survived" "survived" "died"     "survived"
##  [67] "survived" "died"     "survived" "died"     "survived" "died"    
##  [73] "survived" "survived" "survived" "died"     "died"     "survived"
##  [79] "died"     "died"     "survived" "died"     "survived" "survived"
##  [85] "died"     "survived" "died"     "survived" "survived" "survived"
##  [91] "survived" "survived" "died"     "died"     "died"     "died"    
##  [97] "survived" "survived" "died"     "died"
{% endhighlight %}

Similarly, we can generate a set of values from the red light (assuming,
again, that these are equivalent):


{% highlight r %}
red <- sample(c("survived", "died"), size=100, replace=TRUE)
red
{% endhighlight %}



{% highlight text %}
##   [1] "died"     "survived" "survived" "died"     "died"     "survived"
##   [7] "survived" "survived" "died"     "died"     "died"     "died"    
##  [13] "survived" "survived" "survived" "survived" "died"     "survived"
##  [19] "survived" "died"     "died"     "survived" "survived" "survived"
##  [25] "died"     "survived" "died"     "survived" "survived" "died"    
##  [31] "died"     "survived" "survived" "died"     "died"     "died"    
##  [37] "died"     "died"     "died"     "died"     "died"     "died"    
##  [43] "survived" "survived" "died"     "survived" "survived" "died"    
##  [49] "survived" "died"     "died"     "died"     "survived" "survived"
##  [55] "died"     "survived" "died"     "survived" "survived" "survived"
##  [61] "survived" "died"     "survived" "died"     "died"     "survived"
##  [67] "survived" "survived" "died"     "survived" "died"     "died"    
##  [73] "died"     "survived" "survived" "died"     "died"     "died"    
##  [79] "died"     "died"     "survived" "survived" "died"     "died"    
##  [85] "died"     "survived" "died"     "died"     "died"     "died"    
##  [91] "died"     "survived" "survived" "died"     "survived" "died"    
##  [97] "survived" "died"     "survived" "died"
{% endhighlight %}

Then, the proportion of plants under a white light that survived is:


{% highlight r %}
phat_w <- mean(white == "survived") * 100
phat_w
{% endhighlight %}



{% highlight text %}
## [1] 52
{% endhighlight %}

And the proportion of plants under a red light that survived is:


{% highlight r %}
phat_r <- mean(red == "survived") * 100
phat_r
{% endhighlight %}



{% highlight text %}
## [1] 46
{% endhighlight %}

And the difference is given by:


{% highlight r %}
dhat <- phat_w - phat_r
dhat
{% endhighlight %}



{% highlight text %}
## [1] 6
{% endhighlight %}

We could compare our observed value of $\widehat{D}$ to this simulated
value, but it would be better to have **R** generate thousands of
examples for us and compare our $\widehat{D}$ to all of these samples.
We can do this $10,000$ times with the following code:


{% highlight r %}
N <- 10000
dhat_set <- rep(0, 10000)
for (i in seq_len(N))
{
  white <- sample(c("survived", "died"), size=100, replace=TRUE)
  phat_w <- mean(white == "survived") * 100

  red <- sample(c("survived", "died"), size=100, replace=TRUE)
  phat_r <- mean(red == "survived") * 100

  dhat <- phat_w - phat_r
  dhat_set[i] <- dhat
}
{% endhighlight %}

We can see the first 100 values here:


{% highlight r %}
dhat_set[1:100]
{% endhighlight %}



{% highlight text %}
##   [1]   5   9  -5   1 -10   1  10 -10  -8  -1   3   1   5   1   7  -8  -6
##  [18] -14  -5  -2   2   5   6  -7   6  -5   7   4  -2   0  -5  -5  -6 -16
##  [35]  -7  19   2 -12   4   1  -7  -9   4   5   6   1 -18   2   4  10   1
##  [52]   1  -2   0   2   8  -2   1 -12   0   6   1 -16   4  -1  -1  10   4
##  [69]  -1   1   4  -8   5   4  -3  -4  -1  -4  -1   0   3  11  -6   3   8
##  [86] -15  -2  -2  -8  -8   4   5  -5  -3   2   8 -10   3 -10   7
{% endhighlight %}

And visualize them in the following plot (we will learn later how
to make such graphics):

<img src="../assets/class02/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="100%" />

Now, how extreme is the value of $\widehat{D} = 10$ that we observed in our
actual experiment? In other words, what proportion of tests are in the black
region in this plot:

<img src="../assets/class02/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="100%" />

We can compute this using:


{% highlight r %}
mean(dhat_set >= 10) * 100
{% endhighlight %}



{% highlight text %}
## [1] 8.82
{% endhighlight %}

So our observed value of $10$ could occur just by chance about 8.8%
even if there is no real difference between the lighting choice. This
is not particularly strong evidence that blue lights kill pea plants
more than white light.

### Summary

We have introduced a lot of new information today. I primarily want
you to be familiar with the general approach and have a high-level
understanding of the goal of inference for 2x2 contingency tables.
Next class we will go through the exact same process but introduce
the formal language of statistical inference.

