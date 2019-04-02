---
title: "Class 20: Relational Data"
author: "Taylor Arnold"
output: html_notebook
---





## Airline data

Today we will once again look at the NYC flights dataset:


{% highlight r %}
flights <- read_csv("https://statsmaths.github.io/stat_data/flights.csv")
{% endhighlight %}



{% highlight text %}
## Parsed with column specification:
## cols(
##   year = col_double(),
##   month = col_double(),
##   day = col_double(),
##   dep_time = col_double(),
##   sched_dep_time = col_double(),
##   dep_delay = col_double(),
##   arr_time = col_double(),
##   sched_arr_time = col_double(),
##   arr_delay = col_double(),
##   carrier = col_character(),
##   flight = col_double(),
##   tailnum = col_character(),
##   origin = col_character(),
##   dest = col_character(),
##   air_time = col_double(),
##   distance = col_double(),
##   hour = col_double(),
##   minute = col_double(),
##   time_hour = col_datetime(format = "")
## )
{% endhighlight %}


{% highlight r %}
flights <- select(flights, year:day, hour, tailnum, carrier)
{% endhighlight %}


{% highlight r %}
flights
{% endhighlight %}



{% highlight text %}
## # A tibble: 327,346 x 6
##     year month   day  hour tailnum carrier
##    <dbl> <dbl> <dbl> <dbl> <chr>   <chr>  
##  1  2013     1     1     5 N14228  UA     
##  2  2013     1     1     5 N24211  UA     
##  3  2013     1     1     5 N619AA  AA     
##  4  2013     1     1     5 N804JB  B6     
##  5  2013     1     1     6 N668DN  DL     
##  6  2013     1     1     5 N39463  UA     
##  7  2013     1     1     6 N516JB  B6     
##  8  2013     1     1     6 N829AS  EV     
##  9  2013     1     1     6 N593JB  B6     
## 10  2013     1     1     6 N3ALAA  AA     
## # … with 327,336 more rows
{% endhighlight %}

This time we will also look at another table that describes each of the
airlines.


{% highlight r %}
airlines <- read_csv("https://statsmaths.github.io/stat_data/f_airlines.csv")
{% endhighlight %}



{% highlight text %}
## Parsed with column specification:
## cols(
##   carrier = col_character(),
##   name = col_character()
## )
{% endhighlight %}



{% highlight r %}
airlines
{% endhighlight %}



{% highlight text %}
## # A tibble: 16 x 2
##    carrier name                       
##    <chr>   <chr>                      
##  1 9E      Endeavor Air Inc.          
##  2 AA      American Airlines Inc.     
##  3 AS      Alaska Airlines Inc.       
##  4 B6      JetBlue Airways            
##  5 DL      Delta Air Lines Inc.       
##  6 EV      ExpressJet Airlines Inc.   
##  7 F9      Frontier Airlines Inc.     
##  8 FL      AirTran Airways Corporation
##  9 HA      Hawaiian Airlines Inc.     
## 10 MQ      Envoy Air                  
## 11 OO      SkyWest Airlines Inc.      
## 12 UA      United Air Lines Inc.      
## 13 US      US Airways Inc.            
## 14 VX      Virgin America             
## 15 WN      Southwest Airlines Co.     
## 16 YV      Mesa Airlines Inc.
{% endhighlight %}

These two tables are connected by way of a common shared
variable, known in database theory as *keys*. A key may be
a single variable or a collection of variables (known as
a composite key).

In order to combine them, by matching up along keys, we can make use of the
function `left_join`. Here we see that it returns a new table with the same
number of rows as the `flights_sml` dataset but now with the full name of the
carrier:




{% highlight r %}
left_join(flights, airlines, by = "carrier")
{% endhighlight %}



{% highlight text %}
## # A tibble: 327,346 x 7
##     year month   day  hour tailnum carrier name                    
##    <dbl> <dbl> <dbl> <dbl> <chr>   <chr>   <chr>                   
##  1  2013     1     1     5 N14228  UA      United Air Lines Inc.   
##  2  2013     1     1     5 N24211  UA      United Air Lines Inc.   
##  3  2013     1     1     5 N619AA  AA      American Airlines Inc.  
##  4  2013     1     1     5 N804JB  B6      JetBlue Airways         
##  5  2013     1     1     6 N668DN  DL      Delta Air Lines Inc.    
##  6  2013     1     1     5 N39463  UA      United Air Lines Inc.   
##  7  2013     1     1     6 N516JB  B6      JetBlue Airways         
##  8  2013     1     1     6 N829AS  EV      ExpressJet Airlines Inc.
##  9  2013     1     1     6 N593JB  B6      JetBlue Airways         
## 10  2013     1     1     6 N3ALAA  AA      American Airlines Inc.  
## # … with 327,336 more rows
{% endhighlight %}

And the resulting dataset combines all of the variables by the common key.

