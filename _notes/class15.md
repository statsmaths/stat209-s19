---
title: "Class 15: Annotating Data Visualizations"
author: "Taylor Arnold"
output: html
---





## Learning Objectives

- Review elements in the Grammar of Graphics
- Manually annotate of data visualizations with text, lines, and points

## GapMinder Data

For today's notes, we will once again use the `gapminder_2007` dataset:


{% highlight r %}
gapminder_2007 <- read_csv("https://statsmaths.github.io/stat_data/gapminder_2007.csv")
{% endhighlight %}

## Labels

We can add labels to the plot by adding new layers to the plot:

- `xlab("text for the x-axis")`
- `ylab("text for the y-axis")`
- `ggtitle("text for the title/top of the plot")`
- `labs(size = "label for the size legend")`

Let's add these to the current plot:


{% highlight r %}
ggplot(data=gapminder_2007, aes(x=gdp_per_cap, y=life_exp)) +
  geom_point(aes(size=pop)) +
  xlab("Gross domestic product per capita (USD)") +
  ylab("Life expectancy at birth (years)") +
  labs(size = "Population") +
  ggtitle("Macroeconomic variables by country (2007)")
{% endhighlight %}

<img src="../assets/class15/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" width="100%" />

Do not feel that you need to add complex labels to plots as we are doing
an exploratory analysis. However, when presenting plots in a report these
should certainly be used to clarify the plot to the audience or readers.

## Annotations

While we can use graphics simply for our own exploratory work, they can
often also be used to make visual arguments. That's the case, for example,
whenever we use a graphic in a report or presentation. In order to strengthen
a visual argument it is useful to add manual annotations to the plot to help
explain our main points.

In order to add manual annotations, we will use the function `annotate`.
For each annotation, simply add another layer. The exact syntax differs
based on whether we want a point:


{% highlight r %}
annotate("point", x = [X COORDINATE], y = [Y COORDINATE], [OTHER OPTIONS])
{% endhighlight %}

Text:


{% highlight r %}
annotate("text", x = [X COORDINATE], y = [Y COORDINATE], label = [TEXT],
         [OTHER OPTIONS])
{% endhighlight %}

Or a rect(angle):


{% highlight r %}
annotate("rect", xmin = [VALUE], xmax = [VALUE], ymin = [VALUE],
                 ymax = [VALUE], [OTHER OPTIONS])
{% endhighlight %}

For example, to add the current life expectancy (79.1) and GDP (51736) of
Virginia onto the plot, I would do this:


{% highlight r %}
ggplot(data=gapminder_2007, aes(x=gdp_per_cap, y=life_exp)) +
  geom_point(aes(size=pop)) +
  annotate("point", x = 51736, y = 79.1, color = "red", size = 3)
{% endhighlight %}

<img src="../assets/class15/unnamed-chunk-7-1.png" title="plot of chunk unnamed-chunk-7" alt="plot of chunk unnamed-chunk-7" width="100%" />

Similarly, I could label the US on the plot:


{% highlight r %}
ggplot(data=gapminder_2007, aes(x=gdp_per_cap, y=life_exp)) +
  geom_point(aes(size=pop, color = continent)) +
  annotate("text", x = 42950, y = 76.7, label = "USA")
{% endhighlight %}

<img src="../assets/class15/unnamed-chunk-8-1.png" title="plot of chunk unnamed-chunk-8" alt="plot of chunk unnamed-chunk-8" width="100%" />

Or, highlight the healthy and wealthy part of the plot:


{% highlight r %}
ggplot(data=gapminder_2007, aes(x=gdp_per_cap, y=life_exp)) +
  annotate("rect", xmin = 30000, xmax = 50000,
                   ymin = 75, ymax = 85,
                   fill = "purple", alpha = 0.1) +
  geom_point(aes(size=pop)) +
  annotate("text", x = 40000, y = 76,
           label = "Healthiest and Wealthiest Countries")
{% endhighlight %}

<img src="../assets/class15/unnamed-chunk-9-1.png" title="plot of chunk unnamed-chunk-9" alt="plot of chunk unnamed-chunk-9" width="100%" />

Notice here that I set the aesthetics `fill` (that the filled color
of the rectangle) and `alpha` (how opaque the rectangle is); I also
put the rectangle first so that the points were plotted behind the
rectangle.

