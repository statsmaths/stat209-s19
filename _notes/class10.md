---
title: "Class 10: Logistic Regression"
author: "Taylor Arnold"
output: html_document
---



### Learning Objectives

-

## Odds and Log Odds

Looking back through your notes, you'll notice that we have not yet worked
with an inference problem where the response (DV) variable is categorical and
the effect (IV) variable is continuous. To do this, we will make use of logistic
regression. This is the final hypothesis test that we will need this semester.

I have saved logistic regression for last because it is defined in terms of
a slightly more complicated quantity. Assume for a moment that we have a
variable p that tells us the probability of observing a particular event. The
**odds** of this event is the ratio of p divided by 1-p (the probability of
not observing p):

$$ \text{odds} = \frac{p}{1-p} $$

This quantity is often used in gambling contexts. Notice that an odds of 1
indicates that the probability is equal to 0.5; it is just as likely to observer
the event as it is to not observe the event.

The **log odds** is simply the logarithm of the odds:

$$\text{log odds} = \log \left( \frac{p}{1-p} \right) $$

Logistic regression tries to model the logs odds of an event. Specifically, it
tries to build a model that look like:

$$\text{log odds} = \alpha + \beta \cdot X $$

For some variable X. Why model the log odds and not the probability itself?
The probability can only be between 0 and 1, whereas the log odds can range
over and real number. The right-hand side above could take on any value (depending
on the inputs X), and so it is much more mathematically sound to model the
log odds rather than the probability directly.

## Logistic regression











