---
title: "Class 05: Assumptions and Alternative tests"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

- Distinguish between **observational** and **experimental**
studies and the conclusions that can be derived from them.
- Understand the assumptions behind the Z-test for difference in proportions
and common alternatives (Fisher's Exact Test and the Chi-squared Test).

## Causal inference

So far we have focused on a simple data collection method with two input
categories (independent variable / treatment) and two output categories
(dependent variable / effect). You should understand how to organize data
from such an experiment, how to read the data into R, and how to run and
interpret a statistical inference test for whether there is a relationship
between the independent variable and the distribution of categories in the
dependent variable. I want to briefly discuss here what practical results
we can take away from such an analysis.

Roughly speaking, there are two methods by which we can collection a dataset.
The first matches the simulated experiment with pea plants and is known as
an **experimental study**. In this scenario the data has been generated such
that which the explanatory variable of interest is selected at random. That
is, the input variable is applied randomly as a result of the experiment,
such as the use of different color lights in the growth of pea plants. In
theory, nothing other than the light used differentiates the two groups of
plants. In contrast, an **observational study** draws inferences from a sample
to a population where the explanatory variable is not under the control of the
researcher. This is similar to the Wikipedia dataset that we collection.
Reasons for using an observational study include ethical concerns, logistical
constraints, or taking a *sample of convenience*.

The distinction between study types is very important because casual
relationships can only be identified through the use of experimental studies.
In the pea plant example (at least in theory, if it was run correctly), the
**only** difference between the two groups is the light type used. Therefore,
if there is a true difference in the death rate of the plants it __must be
caused__ by the light. In an observational study, there are many other
explanations.

Assume for example that we randomly take 1000 U.S. households with children
under the age of 18. We will collect data according to a 2x2 contingency
table with a independent variable indicating if the household has single or
multiple parents present and the dependent variable is whether the children
receive subsidized school lunches. Assume that a Z-test provides a p-value of
0.00001, indicating that it is unlikely that these two variables are
unrelated, and the proportion of subsidised school lunches is higher in the
single parent household set. There are at least three explanations for this:

- being in a single-parent household *directly causes* the probability of
receiving a lunch subsidy to increase
- having a lunch subsidy *directly causes* a child to have a higher chance
of being in a single parent household
- a host of other factors (structural inequalities, regional effect, race,
education, language, ect.) both effect the parental composition and economic
conditions of U.S. households

We have no mathematical way of distinguishing these possibilities with the
available data (though logically the last one is the most likely). Therefore,
it would not, for example, make sense to assume from this data that we can
directly change economic conditions through policies that attempt to decrease
the proportion of single family households.

Later in the course we will further discuss how we can attempt to recover some
form of causality through observational studies. However, there is no way to
entirely establish the precise causal conclusions that we can make from true
experimental designs.

## Z-test for proportions: assumptions

We have not yet gone much into the assumptions that are required for the
Z-test for proportions to be valid. In part, this is because a precise
description requires the full mathematical apparatus of probability theory.
We can, however, still describe the requirements qualitatively. There are
three things that need to hold for the Z-test for proportions to be
statistically valid (in other words, for the probability statement behind the
p-value under the Null hypothesis to be true):

1. the data were sampled uniformly from the population of interest
2. the response variable values are unrelated (i.e., independent) to one another
3. the sample sizes are reasonably large (usually, we want at least 10 observations)

Can you think of ways that the first assumption might be broken? How about
the second? The third assumption is needed in order for a condition known as
the central limit theorem to be applied. More on this in our second unit.

In truth, the first two typically only approximately hold, but we hope that
the sampled data are uniform *enough* and independent *enough* to be relatively
insignificant to the result. How to measure this is where the qualitative part
of statistics comes into play.

## Z-test for proportions: and alternatives

There are two common alternatives to the Z-test for proportions that you will
see in scientific and social scientific literature. Typically the results will
be similar for all three variations, but it is useful to understand the
differences in the underlying assumptions.

**Fisher's Exact Test** is a variation that assumes that the "margin sums of the
contingency table are fixed and known" ahead of the experiment. Here is a
full description of the original experiment from which is comes:

> In the design of experiments in statistics, the lady tasting tea is a
> randomized experiment devised by Ronald Fisher and reported in his book
> The Design of Experiments (1935). The experiment is the original exposition
> of Fisher's notion of a null hypothesis...

> The lady in question (Muriel Bristol) claimed to be able to tell whether
> the tea or the milk was added first to a cup. Fisher proposed to give her
> eight cups, four of each variety, in random order. One could then ask what
> the probability was for her getting the specific number of cups she
> identified correct, but just by chance.

> The experiment provided the lady with 8 randomly ordered cups of tea—4
> prepared by first adding milk, 4 prepared by first adding the tea. She was
> to select the 4 cups prepared by one method. This offered the lady the
> advantage of judging cups by comparison. She was fully informed of the
> experimental method.

Despite the conditions of this test being incredibly rare, the use of the test
is quite common in medical studies. You can apply it in R using the function
`tmod_fisher_test` in exactly the same way as the function
`tmod_z_test_prop`.

In contrast, the chi-squared test assumes that both categorical variables are
randomly determined as a result of the experiment. That is, unlike our plant
and Wikipedia experiments, the number of observations in both independent
variable categories are unknown. This more closely matches the sample of 1000
U.S. households example above. While certainly not required, the chi-squared
test assumptions are more likely to hold in observational studies compared to
experimental studies. You can run this test using `tmod_chi_squared_test`.

Today's lab will give you a chance to experiment with all three of these
tests. Note that science and social science research commonly conflates these
tests and you should not put much faith on their "correct" usage. Also, the
results are generally not too far off between each of the tests.

