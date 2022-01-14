---
title: i am learning about... clean_names
author: ''
date: '2021-06-16'
slug: []
categories: 
- i am learning about...
- clean_names
- janitor
tags: 
- i am learning about...
- clean_names
- janitor
---




# tell us 

The `clean_names()` function from the janitor package is my FAVOURITE function. Most of the time psychology R users don't think very hard about what they are called variables when they enter their data. So when it gets into R, variable names tend to be SUPER long and inconsistently formatted. This is a problem, because in R you have to type out variable names A LOT so it is much easier if they have a consistent format. 

The clean_names() function from janitor will make all your variable names consistent in one line of code. 


# show us


### install and load packages

You get access to the `clean_names()` function when you load the janitor package. Here we are also loading `tidyverse` to read in some data, and `here` to tell R where our data lives. 


```r
library(tidyverse)
library(janitor)
library(here)
```

### get some data


```r
fav_things <- read_csv("my_favourite_things.csv") %>%
  select(1:5)

glimpse(fav_things)
```

```
## Rows: 113
## Columns: 5
## $ Timestamp               <chr> "2/11/19 20:57", "3/11/19 8:51", "3/11/19 8:58…
## $ `Raindrops on roses`    <dbl> 5, 6, 4, 5, 7, 5, 6, 5, 5, 3, 1, 7, 5, 6, 5, 6…
## $ `Whiskers on kittens`   <dbl> 3, 6, 7, 7, 7, 4, 6, 3, 7, 5, 7, 3, 7, 4, 7, 6…
## $ `Bright copper kettles` <dbl> 4, 4, 6, 3, 5, 3, 5, 4, 4, 2, 4, 5, 5, 3, 2, 6…
## $ `Warm woollen mittens`  <dbl> 2, 4, 3, 1, 6, 7, 7, 6, 5, 6, 7, 6, 3, 6, 2, 6…
```

### use the function 

You will agree that these variable names are TERRIBLE. They are going to be a gigantic pain in the bum to type out, both because they are REALLY long but also because they are inconsistently formatted. Lets clean them up!


```r
clean_fav_things <- fav_things %>%
  clean_names()

glimpse(clean_fav_things)
```

```
## Rows: 113
## Columns: 5
## $ timestamp             <chr> "2/11/19 20:57", "3/11/19 8:51", "3/11/19 8:58",…
## $ raindrops_on_roses    <dbl> 5, 6, 4, 5, 7, 5, 6, 5, 5, 3, 1, 7, 5, 6, 5, 6, …
## $ whiskers_on_kittens   <dbl> 3, 6, 7, 7, 7, 4, 6, 3, 7, 5, 7, 3, 7, 4, 7, 6, …
## $ bright_copper_kettles <dbl> 4, 4, 6, 3, 5, 3, 5, 4, 4, 2, 4, 5, 5, 3, 2, 6, …
## $ warm_woollen_mittens  <dbl> 2, 4, 3, 1, 6, 7, 7, 6, 5, 6, 7, 6, 3, 6, 2, 6, …
```
So `clean_names()` automagically converts all your variable names to lower case and puts underscores in the gaps. These variable names are still WAY too long, but at least they are all in the same format. 

It also does clever things like convert variable names that have % in them to "percent"- very handy. 

Also, while the default behaviour is to convert all variable names to "snake_case" (i.e. lower case with underscores), if you don't like that, you can choose other case options. 

Here are the options from the [R documentation](https://www.rdocumentation.org/packages/janitor/versions/1.2.0/topics/clean_names)

```
clean_names(dat, case = c("snake", "lower_camel",
  "upper_camel", "screaming_snake", "lower_upper", "upper_lower",
  "all_caps", "small_camel", "big_camel", "old_janitor", "parsed", "mixed",
  "none"))
  
```

For example, if you would like your variable names to YELL at you.... you can choose screaming_snake.


```r
screaming_snake <- fav_things %>%
  select(1:5) %>%
  clean_names("screaming_snake") 

glimpse(screaming_snake)
```

```
## Rows: 113
## Columns: 5
## $ TIMESTAMP             <chr> "2/11/19 20:57", "3/11/19 8:51", "3/11/19 8:58",…
## $ RAINDROPS_ON_ROSES    <dbl> 5, 6, 4, 5, 7, 5, 6, 5, 5, 3, 1, 7, 5, 6, 5, 6, …
## $ WHISKERS_ON_KITTENS   <dbl> 3, 6, 7, 7, 7, 4, 6, 3, 7, 5, 7, 3, 7, 4, 7, 6, …
## $ BRIGHT_COPPER_KETTLES <dbl> 4, 4, 6, 3, 5, 3, 5, 4, 4, 2, 4, 5, 5, 3, 2, 6, …
## $ WARM_WOOLLEN_MITTENS  <dbl> 2, 4, 3, 1, 6, 7, 7, 6, 5, 6, 7, 6, 3, 6, 2, 6, …
```


# tips and tricks

The janitor package [vignette](https://cran.r-project.org/web/packages/janitor/vignettes/janitor.html) has good coverage of how to use the clean_names function and Garth Tarr has a nice summary at [this site](https://garthtarr.github.io/meatR/janitor.html). We found the [RDocumentation](https://www.rdocumentation.org/packages/janitor/versions/1.2.0/topics/clean_names) useful to work out the argument options for different types of case preferences. You can find great art from Allison Horst re clean_names and other R functions [here](https://github.com/allisonhorst/stats-illustrations) 





