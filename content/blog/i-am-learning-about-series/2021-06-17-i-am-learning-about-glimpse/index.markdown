---
title: i am learning about... glimpse
author: ''
date: '2021-06-17'
slug: []
categories: 
- i am learning about...
- glimpse
- pillar
tags: 
- i am learning about...
- glimpse
- pillar
---

# tell us 

When we saw `pillar::glimpse()` in the list we thought that it was something that we hadn't used before, BUT... turns out, the `pillar` package (which contains the `glimpse()` function is part of the tidyverse. 

The `glimpse()` function is SUPER useful when you want to get a quick look at the variables in your dataframe and what kind of data R thinks they are. 

We use it a lot when we are adding new variables or changing the kind of data of a particular variable, just as a sanity check to make sure that R has done what we asked it to. 


# show us


### install and load packages

You get access to the `glimpse()` function when you load the tidyverse (the pillar package is auto-loaded in the background)


```r
library(tidyverse)
library(palmerpenguins)
```

### get some data


```r
penguins <- penguins
```

### use the function 

You can glimpse ALL the variables... 


```r
glimpse(penguins)
```

```
## Rows: 344
## Columns: 8
## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
## $ sex               <fct> male, female, female, NA, female, male, female, male…
## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
```

... or if you have TOO MANY VARIABLES, you can pick just one. 


```r
glimpse(penguins$island)
```

```
##  Factor w/ 3 levels "Biscoe","Dream",..: 3 3 3 3 3 3 3 3 3 3 ...
```


# tips and tricks

As always, the tidyverse vignettes are super useful, the [vignette for pillar is here](glimpse(nycflights13::flights)). 



