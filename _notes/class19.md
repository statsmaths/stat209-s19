---
title: "Class 19: Collecting Relational Data"
author: "Taylor Arnold"
output: html_notebook
---





In order to let you get started on the final project, I have reordered the
typical order of my notes on data manipulation. Today we are going to see 
how to collect and manipulate data stored in multiple tables.

## Tidy data

## Three principles

We have already discussed at length how to store tablular datasets and 
some good practices for naming variables. As datasets become more complex,
there are some additional concerns that arise. All of the principles of
constructing a dataset (equivalently, a database) could easily fill a
whole course. Here are three principles that get us on the right track:

- determine the objects of study; each of these gets its own table,
and each example gets its own row; **movies**, **actors**,
**actor-movie links**
- information about an object of study should be put on the relevant
table; the information should remain internally consistent regardless of
changes made within a data table
- each column should be indivisible and the variable type clear;
for example, budget should not include the dollar sign, if needed
create a new column; name columns with no spaces or special characters

If you would like to know about specifics of these principals I highly
recommend the Hadley Wickhams "Tidy Data" paper:

- ["Tidy Data" (2014)](http://vita.had.co.nz/papers/tidy-data.html)

The Tidy Data chapter of R for Data Science covers several useful
functions the show to convert a dataset that has already been collected
into a tidy format:

- [Tidy Data R4DS](http://r4ds.had.co.nz/tidy-data.html)

To better explain these principles, let's look at several tables from the
airline dataset.

## Airline data

Today we will once again look at the NYC flights dataset:


{% highlight r %}
flights <- read_csv("https://statsmaths.github.io/stat_data/flights.csv")
flights
{% endhighlight %}



{% highlight text %}
## # A tibble: 327,346 x 19
##     year month   day dep_time sched_dep_time dep_delay arr_time
##    <dbl> <dbl> <dbl>    <dbl>          <dbl>     <dbl>    <dbl>
##  1  2013     1     1      517            515         2      830
##  2  2013     1     1      533            529         4      850
##  3  2013     1     1      542            540         2      923
##  4  2013     1     1      544            545        -1     1004
##  5  2013     1     1      554            600        -6      812
##  6  2013     1     1      554            558        -4      740
##  7  2013     1     1      555            600        -5      913
##  8  2013     1     1      557            600        -3      709
##  9  2013     1     1      557            600        -3      838
## 10  2013     1     1      558            600        -2      753
## # … with 327,336 more rows, and 12 more variables: sched_arr_time <dbl>,
## #   arr_delay <dbl>, carrier <chr>, flight <dbl>, tailnum <chr>,
## #   origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>, hour <dbl>,
## #   minute <dbl>, time_hour <dttm>
{% endhighlight %}

This time we will also look at another table that describes each of the
airlines.


{% highlight r %}
airlines <- read_csv("https://statsmaths.github.io/stat_data/f_airlines.csv")
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

Another describing the planes:


{% highlight r %}
planes <- read_csv("https://statsmaths.github.io/stat_data/f_planes.csv")
planes
{% endhighlight %}



{% highlight text %}
## # A tibble: 3,322 x 9
##    tailnum  year type       manufacturer  model  engines seats speed engine
##    <chr>   <dbl> <chr>      <chr>         <chr>    <dbl> <dbl> <dbl> <chr> 
##  1 N10156   2004 Fixed win… EMBRAER       EMB-1…       2    55    NA Turbo…
##  2 N102UW   1998 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  3 N103US   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  4 N104UW   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  5 N10575   2002 Fixed win… EMBRAER       EMB-1…       2    55    NA Turbo…
##  6 N105UW   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  7 N107US   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  8 N108UW   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
##  9 N109UW   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
## 10 N110UW   1999 Fixed win… AIRBUS INDUS… A320-…       2   182    NA Turbo…
## # … with 3,312 more rows
{% endhighlight %}

The airports:


{% highlight r %}
airports <- read_csv("https://statsmaths.github.io/stat_data/f_airports.csv")
airports
{% endhighlight %}



{% highlight text %}
## # A tibble: 1,458 x 8
##    faa   name                    lat    lon   alt    tz dst   tzone        
##    <chr> <chr>                 <dbl>  <dbl> <dbl> <dbl> <chr> <chr>        
##  1 04G   Lansdowne Airport      41.1  -80.6  1044    -5 A     America/New_…
##  2 06A   Moton Field Municipa…  32.5  -85.7   264    -6 A     America/Chic…
##  3 06C   Schaumburg Regional    42.0  -88.1   801    -6 A     America/Chic…
##  4 06N   Randall Airport        41.4  -74.4   523    -5 A     America/New_…
##  5 09J   Jekyll Island Airport  31.1  -81.4    11    -5 A     America/New_…
##  6 0A9   Elizabethton Municip…  36.4  -82.2  1593    -5 A     America/New_…
##  7 0G6   Williams County Airp…  41.5  -84.5   730    -5 A     America/New_…
##  8 0G7   Finger Lakes Regiona…  42.9  -76.8   492    -5 A     America/New_…
##  9 0P2   Shoestring Aviation …  39.8  -76.6  1000    -5 U     America/New_…
## 10 0S9   Jefferson County Intl  48.1 -123.    108    -8 A     America/Los_…
## # … with 1,448 more rows
{% endhighlight %}

And the weather:


{% highlight r %}
weather <- read_csv("https://statsmaths.github.io/stat_data/f_weather.csv")
weather
{% endhighlight %}



{% highlight text %}
## # A tibble: 26,130 x 15
##    origin  year month   day  hour  temp  dewp humid wind_dir wind_speed
##    <chr>  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>    <dbl>      <dbl>
##  1 EWR     2013     1     1     0  37.0  21.9  54.0      230      10.4 
##  2 EWR     2013     1     1     1  37.0  21.9  54.0      230      13.8 
##  3 EWR     2013     1     1     2  37.9  21.9  52.1      230      12.7 
##  4 EWR     2013     1     1     3  37.9  23    54.5      230      13.8 
##  5 EWR     2013     1     1     4  37.9  24.1  57.0      240      15.0 
##  6 EWR     2013     1     1     6  39.0  26.1  59.4      270      10.4 
##  7 EWR     2013     1     1     7  39.0  27.0  61.6      250       8.06
##  8 EWR     2013     1     1     8  39.0  28.0  64.4      240      11.5 
##  9 EWR     2013     1     1     9  39.9  28.0  62.2      250      12.7 
## 10 EWR     2013     1     1    10  39.0  28.0  64.4      260      12.7 
## # … with 26,120 more rows, and 5 more variables: wind_gust <dbl>,
## #   precip <dbl>, pressure <dbl>, visib <dbl>, time_hour <dttm>
{% endhighlight %}

Each of these tables captures a specific unit of observation. Information
about a unit of observation is only included in a specific table. Note that,
for example, including the origin airport in the flights table is not an
error: this **is** information about the flight. However, if we put in
information about the timezone of the flight, this would break the tidy
data principles.

## Two table verbs

Having data places across different tables is useful, but often we need to
put the data back together before using it. Notice that the various tables
are connected by way of common shared variables, known in database theory
as *keys*. A key may be a single variable or a collection of variables
(a composite key).

In order to combine them, by matching up along keys, we can make use of the
function `left_join`. Here we see that it returns a new table with the same
number of rows as the `flights` dataset but now with the full name of the
carrier. I'll make use of a pipe to reduce the number of variables to make it
easier to see:


{% highlight r %}
left_join(flights, airlines, by = "carrier") %>%
  select(year, month, day, origin, dest, carrier, name)
{% endhighlight %}



{% highlight text %}
## # A tibble: 327,346 x 7
##     year month   day origin dest  carrier name                    
##    <dbl> <dbl> <dbl> <chr>  <chr> <chr>   <chr>                   
##  1  2013     1     1 EWR    IAH   UA      United Air Lines Inc.   
##  2  2013     1     1 LGA    IAH   UA      United Air Lines Inc.   
##  3  2013     1     1 JFK    MIA   AA      American Airlines Inc.  
##  4  2013     1     1 JFK    BQN   B6      JetBlue Airways         
##  5  2013     1     1 LGA    ATL   DL      Delta Air Lines Inc.    
##  6  2013     1     1 EWR    ORD   UA      United Air Lines Inc.   
##  7  2013     1     1 EWR    FLL   B6      JetBlue Airways         
##  8  2013     1     1 LGA    IAD   EV      ExpressJet Airlines Inc.
##  9  2013     1     1 JFK    MCO   B6      JetBlue Airways         
## 10  2013     1     1 LGA    ORD   AA      American Airlines Inc.  
## # … with 327,336 more rows
{% endhighlight %}

The resulting dataset combines all of the variables by the common key.

### Composite keys

Recall that a key may consist of multiple variables. To
join on many variables at once, provide a vector of all the names
to the option `by`:


{% highlight r %}
flights_sml <- flights %>%
  select(year:day, hour, origin, dest)

inner_join(flights_sml, weather,
          by = c("year", "month", "day", "hour", "origin"))
{% endhighlight %}



{% highlight text %}
## # A tibble: 326,160 x 16
##     year month   day  hour origin dest   temp  dewp humid wind_dir
##    <dbl> <dbl> <dbl> <dbl> <chr>  <chr> <dbl> <dbl> <dbl>    <dbl>
##  1  2013     1     1     6 LGA    ATL    39.9  26.1  57.3      260
##  2  2013     1     1     6 EWR    FLL    39.0  26.1  59.4      270
##  3  2013     1     1     6 LGA    IAD    39.9  26.1  57.3      260
##  4  2013     1     1     6 JFK    MCO    39.0  26.1  59.4      260
##  5  2013     1     1     6 LGA    ORD    39.9  26.1  57.3      260
##  6  2013     1     1     6 JFK    PBI    39.0  26.1  59.4      260
##  7  2013     1     1     6 JFK    TPA    39.0  26.1  59.4      260
##  8  2013     1     1     6 JFK    LAX    39.0  26.1  59.4      260
##  9  2013     1     1     6 EWR    SFO    39.0  26.1  59.4      270
## 10  2013     1     1     6 LGA    DFW    39.9  26.1  57.3      260
## # … with 326,150 more rows, and 6 more variables: wind_speed <dbl>,
## #   wind_gust <dbl>, precip <dbl>, pressure <dbl>, visib <dbl>,
## #   time_hour <dttm>
{% endhighlight %}

### Common variables

Sometimes there may be a variable name present in two datasets that
we want to merge together but that has a different meaning in each
dataset. For example, `year` is the year of the flight in the `flights`
dataset but the year of creating in the `airplanes` dataset. If
we join these two, we see that a suffix is added to each variable:


{% highlight r %}
flights_sml <- flights %>%
  select(year:day, tailnum)
inner_join(flights_sml, planes,
          by = c("tailnum"))
{% endhighlight %}



{% highlight text %}
## # A tibble: 279,017 x 12
##    year.x month   day tailnum year.y type  manufacturer model engines seats
##     <dbl> <dbl> <dbl> <chr>    <dbl> <chr> <chr>        <chr>   <dbl> <dbl>
##  1   2013     1     1 N14228    1999 Fixe… BOEING       737-…       2   149
##  2   2013     1     1 N24211    1998 Fixe… BOEING       737-…       2   149
##  3   2013     1     1 N619AA    1990 Fixe… BOEING       757-…       2   178
##  4   2013     1     1 N804JB    2012 Fixe… AIRBUS       A320…       2   200
##  5   2013     1     1 N668DN    1991 Fixe… BOEING       757-…       2   178
##  6   2013     1     1 N39463    2012 Fixe… BOEING       737-…       2   191
##  7   2013     1     1 N516JB    2000 Fixe… AIRBUS INDU… A320…       2   200
##  8   2013     1     1 N829AS    1998 Fixe… CANADAIR     CL-6…       2    55
##  9   2013     1     1 N593JB    2004 Fixe… AIRBUS       A320…       2   200
## 10   2013     1     1 N793JB    2011 Fixe… AIRBUS       A320…       2   200
## # … with 279,007 more rows, and 2 more variables: speed <dbl>,
## #   engine <chr>
{% endhighlight %}

This behavior is fine, but it is better to manually describe
what the suffix should be:


{% highlight r %}
inner_join(flights_sml, planes,
          by = c("tailnum"),
          suffix = c("", "_plane"))
{% endhighlight %}



{% highlight text %}
## # A tibble: 279,017 x 12
##     year month   day tailnum year_plane type  manufacturer model engines
##    <dbl> <dbl> <dbl> <chr>        <dbl> <chr> <chr>        <chr>   <dbl>
##  1  2013     1     1 N14228        1999 Fixe… BOEING       737-…       2
##  2  2013     1     1 N24211        1998 Fixe… BOEING       737-…       2
##  3  2013     1     1 N619AA        1990 Fixe… BOEING       757-…       2
##  4  2013     1     1 N804JB        2012 Fixe… AIRBUS       A320…       2
##  5  2013     1     1 N668DN        1991 Fixe… BOEING       757-…       2
##  6  2013     1     1 N39463        2012 Fixe… BOEING       737-…       2
##  7  2013     1     1 N516JB        2000 Fixe… AIRBUS INDU… A320…       2
##  8  2013     1     1 N829AS        1998 Fixe… CANADAIR     CL-6…       2
##  9  2013     1     1 N593JB        2004 Fixe… AIRBUS       A320…       2
## 10  2013     1     1 N793JB        2011 Fixe… AIRBUS       A320…       2
## # … with 279,007 more rows, and 3 more variables: seats <dbl>,
## #   speed <dbl>, engine <chr>
{% endhighlight %}

### Unspecified key

If we do not specify the key to join with in the `by` option, **dplyr**
will assume that we want to join on all common keys. A warning will be
produced warning which variables were choosen. This can be useful in a
pinch, but generally is a bad idea to rely on.

### Practice (or not)

In the interest of time, we will practice the `left_join` function on the
next lab. We will spend the end of class today discussing the final project.
