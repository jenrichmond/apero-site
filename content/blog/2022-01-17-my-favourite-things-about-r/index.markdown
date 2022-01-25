---
title: my favourite things about R
author: Jen Richmond
date: '2022-01-17'
slug: []
categories:
  - dplyr
  - ggplot
  - janitor
  - datawrangling
tags: []
summary:
  "I am prepping a talk for R-Ladies Sydney about my favourite R things, the packages and functions that end up in every script I write."
---

I am prepping a talk for R-Ladies Sydney about my favourite R things, the packages and functions that end up in every script I write.

``` r
library(tidyverse)
library(here)
library(janitor)
library(lubridate)
library(ggeasy)
library(palmerpenguins)
library(naniar)
library(gt)
```

# Avoid filepath drama

## here::here()

The `here` package makes dealing with file paths and telling R where your work lives really easy. If you work within a R project (always recommended), here::here() defaults to the top level of your project folder. You can refer to everything relative to there and use quotes to specify folder levels. Today I am writing in my blogdown project, so here tells me that I am currently…

``` r
here::here()
```

    ## [1] "/Users/jennyrichmond/Documents/GitHub/apero-site"

When I want to read in some data, I can refer to the location of that data relative to this starting point. In my blogdown site, I’ve put the data in the folder for this particular post (within content/blog/). The nice thing about referring to the location of things relative to the top level of your project, is that it doesn’t matter if you are working in Rmd or R script, on the computer that you wrote the code on or another one, the path doesn’t change.

``` r
practice_penguins <- read_csv(here("content", "blog", "2022-01-17-my-favourite-things-about-r", "practice_penguins.csv"))
```

# Fix variable names

## janitor::clean_names()

I messed up the penguin data to make the variable names a bit ugly so I could demo my favourite function, `clean_names()`. So often little thought is put into naming conventions at the time of data entry and it is really common to be given a dataset that has really longwinded and inconsistently formatted variable names.

``` r
names(practice_penguins)
```

    ## [1] "Species"        "island"         "Bill length"    "bill_depth"    
    ## [5] "flipper length" "Body_Mass"      "Sex"            "year"

In this case there is a mix of upper and lower case, some gaps between words, some underscores. When you are coding, you need to type the names of variables a lot, so it can save you lots of time to make the variable names consistent… enter `clean_names()`

``` r
clean_penguins <- practice_penguins %>%
  clean_names()

names(clean_penguins)
```

    ## [1] "species"        "island"         "bill_length"    "bill_depth"    
    ## [5] "flipper_length" "body_mass"      "sex"            "year"

In one line of code, everything is lower case with underscores in the gaps (aka snake case).

# Count things

## janitor::tabyl()

Often the first thing you want to do in R is count how many observations you have of different type. The `tabyl()` function from janitor works much like the `count()` function, but the output is more concise and user friendly and includes percentages automatically.

``` r
clean_penguins %>%
  tabyl(species) 
```

    ##    species   n   percent
    ##     Adelie 152 0.4418605
    ##  Chinstrap  68 0.1976744
    ##     Gentoo 124 0.3604651

You can count just one variable, or get something a bit like a cross tab with two. There are a series of adorn\_ functions that also allow you to add totals.

``` r
clean_penguins %>%
  tabyl(species, sex) %>%
  adorn_totals() 
```

    ##    species female male NA_
    ##     Adelie     73   73   6
    ##  Chinstrap     34   34   0
    ##     Gentoo     58   61   5
    ##      Total    165  168  11

You can assign the output to a dataframe or pipe into `gt()` to get a nice looking rendered output.

``` r
clean_penguins %>%
  tabyl(species, sex) %>%
  adorn_totals() %>%
  gt()
```

<div id="zhysxcsayv" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zhysxcsayv .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#zhysxcsayv .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zhysxcsayv .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zhysxcsayv .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zhysxcsayv .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zhysxcsayv .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#zhysxcsayv .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#zhysxcsayv .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#zhysxcsayv .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zhysxcsayv .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zhysxcsayv .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 5px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#zhysxcsayv .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#zhysxcsayv .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#zhysxcsayv .gt_from_md > :first-child {
  margin-top: 0;
}

#zhysxcsayv .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zhysxcsayv .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#zhysxcsayv .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#zhysxcsayv .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zhysxcsayv .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#zhysxcsayv .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zhysxcsayv .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zhysxcsayv .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zhysxcsayv .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zhysxcsayv .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zhysxcsayv .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#zhysxcsayv .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#zhysxcsayv .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#zhysxcsayv .gt_left {
  text-align: left;
}

#zhysxcsayv .gt_center {
  text-align: center;
}

#zhysxcsayv .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zhysxcsayv .gt_font_normal {
  font-weight: normal;
}

#zhysxcsayv .gt_font_bold {
  font-weight: bold;
}

#zhysxcsayv .gt_font_italic {
  font-style: italic;
}

#zhysxcsayv .gt_super {
  font-size: 65%;
}

#zhysxcsayv .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">species</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">female</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">male</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">NA_</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">Adelie</td>
<td class="gt_row gt_right">73</td>
<td class="gt_row gt_right">73</td>
<td class="gt_row gt_right">6</td></tr>
    <tr><td class="gt_row gt_left">Chinstrap</td>
<td class="gt_row gt_right">34</td>
<td class="gt_row gt_right">34</td>
<td class="gt_row gt_right">0</td></tr>
    <tr><td class="gt_row gt_left">Gentoo</td>
<td class="gt_row gt_right">58</td>
<td class="gt_row gt_right">61</td>
<td class="gt_row gt_right">5</td></tr>
    <tr><td class="gt_row gt_left">Total</td>
<td class="gt_row gt_right">165</td>
<td class="gt_row gt_right">168</td>
<td class="gt_row gt_right">11</td></tr>
  </tbody>
  
  
</table>
</div>

# Find missing values

## naniar::vis_miss

Sometimes you know there is missing data but it can be difficult to know where it is or what to do about it. The `vis_miss()` function from the naniar\` package helps you see where the missing values are so you can better decide what to do with them.

``` r
naniar::vis_miss(clean_penguins)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

# Make wide data long

## tidyr::pivot_longer()

When we enter data it is usually in wide format. This is problematic when you want to use ggplot, which expects your data to be long. The new pivot functions from `tidyr` make it really easy to switch your data from wide to long (and back again if you need). Here I am selecting just species and the two variables that start with “bill” to make a smaller demo dataset.

``` r
penguin_bill <- clean_penguins %>%
  select(species, starts_with("bill"))

glimpse(penguin_bill)
```

    ## Rows: 344
    ## Columns: 3
    ## $ species     <chr> "Adelie", "Adelie", "Adelie", "Adelie", "Adelie", "Adelie"…
    ## $ bill_length <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, 42.0, …
    ## $ bill_depth  <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, 20.2, …

Technically this bill data is in wide format (it is not the best example but lets run with it). The two columns contain bill measurements, about two different parts of the penguin bill. We could represent this data in long format by making a new column that contained info about which part of the bill we were measuring, and another column with the measurement value.

The `pivot_longer()` function asks you to specify what you want to call the column that will contain what is currently in the variable names (i.e. names_to), what you want to call the column that will contain the values (i.e. values to) and the range of columns that are currently wide that you want to be long.

``` r
long_bill <- penguin_bill %>%
  pivot_longer(names_to = "bill_part", 
               values_to = "measurement",   bill_length:bill_depth)

head(long_bill)
```

    ## # A tibble: 6 × 3
    ##   species bill_part   measurement
    ##   <chr>   <chr>             <dbl>
    ## 1 Adelie  bill_length        39.1
    ## 2 Adelie  bill_depth         18.7
    ## 3 Adelie  bill_length        39.5
    ## 4 Adelie  bill_depth         17.4
    ## 5 Adelie  bill_length        40.3
    ## 6 Adelie  bill_depth         18

# Make ggplot easy

## ggeasy::easy_remove_legend()

Once you have your head around how to construct figures in ggplot, you can spend a lot of time googling how to customise it. The `ggeasy` package contains a whole lot of easy to use wrappers for really common ggplot adjustments. Like removing the legend…the code to remove the legend is p + theme(legend.position = “none”) … or you can use `ggeasy::easy_remove_legend()`

https://www.datanovia.com/en/blog/ggplot-legend-title-position-and-labels/

``` r
long_bill %>% 
  ggplot(aes(x = species, y = measurement, colour = species)) +
  geom_jitter(width = 0.2, alpha = 0.5) +
  facet_wrap(~ bill_part) +
  easy_remove_legend()
```

    ## Warning: Removed 4 rows containing missing values (geom_point).

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-12-1.png" width="672" />

# Make new conditional variables

## dplyr::case_when()

Sometimes you need to compute a new variable based on values in other variables, `case_when()` is your friend. Lets say we were interested in which penguins have extremely long or short bills. Here I am filtering for just the Gentoo penguins, and calculating the mean and sd for bill length. Then I am using `mutate()` to make a new variable and `case_when()` to flag values of bill length than are more than 2sd greater than the mean as “long” and values of bill length that are more than 2sd below the mean as short. The TRUE \~ “ordinary,” puts ordinary in the cells that don’t meet those criteria.

Then we can use `tabyl()` to count how many penguins have extraordinarily long or short bills.

``` r
gentoo <- clean_penguins %>%
  filter(species == "Gentoo") %>%
  select(species, bill_length, sex)

mean_length <- mean(gentoo$bill_length, na.rm = TRUE)
sd_length <- sd(gentoo$bill_length, na.rm = TRUE)

gentoo <- gentoo %>%
  mutate(long_short = case_when(bill_length > mean_length + 2*sd_length ~ "long", 
                         bill_length < mean_length - 2*sd_length ~ "short",
                         TRUE ~ "ordinary"))

gentoo %>% tabyl(long_short)
```

    ##  long_short   n     percent
    ##        long   4 0.032258065
    ##    ordinary 119 0.959677419
    ##       short   1 0.008064516

# Move new variables

## dplyr::relocate()

When using `mutate()` to make a new variable, the default is to add it to the right side of the dataframe. With small datasets that is ok, but when you have lots of variables and you want to check whether the mutate has done what you want, it can be annoying. There is a relatively new function in `dplyr` that allows you to relocate a variable. Here I am moving the long_short variable we just made to the position after bill_length.

``` r
gentoo <- gentoo %>%
  relocate(long_short, .after = bill_length)

glimpse(gentoo)
```

    ## Rows: 124
    ## Columns: 4
    ## $ species     <chr> "Gentoo", "Gentoo", "Gentoo", "Gentoo", "Gentoo", "Gentoo"…
    ## $ bill_length <dbl> 46.1, 50.0, 48.7, 50.0, 47.6, 46.5, 45.4, 46.7, 43.3, 46.8…
    ## $ long_short  <chr> "ordinary", "ordinary", "ordinary", "ordinary", "ordinary"…
    ## $ sex         <chr> "female", "male", "female", "male", "male", "female", "fem…

Lets make a plot to illustrate the variability in Gentoo penguins bill length.

``` r
gentoo %>%
  ggplot(aes(x = sex, y = bill_length, colour = long_short)) + 
  geom_jitter(width = 0.2)
```

    ## Warning: Removed 1 rows containing missing values (geom_point).

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

# Save all Rmd figures to folder

## knitr options fig.path =

You might have noticed that in the default Rmd template there is a chunk at the top that controls how your document knits. The default knit settings have echo = TRUE which makes your code appear in your knitted document along with your output. But you can add other knit settings.

<img src="default.png" width="1111" />

You can add fig.width, fig.height, and fig.path to control how big your plots appear in your knitted document. You can also add fig.path to have your plots be rendered in png format to a folder within your project. And if you want all the ggplots in your document to be the same theme, you can add that as a default.

<img src="custom.png" width="990" />

# Write new data to csv

## readr::write_csv()

My data analysis process often involves reading in raw data, cleaning it up, and then writing it out to csv so that you can read the clean data in to another process (visualisation, modelling). I use `write_csv()` and `here::here()` to write out a csv that can then be used in a different script.

``` r
gentoo %>%
  write_csv("clean_gentoo.csv")
```
