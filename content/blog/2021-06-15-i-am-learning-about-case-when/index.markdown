---
title: i am learning about... case_when
author: ''
date: '2022-01-14'
slug: []
categories: []
tags: []
---

# tell us

`case_when()` is a super handy function from the `dplyr` package, which is part of the tidyverse. `case_when()` and `mutate()` are best friends and together to allow you to make a new variable that is based on conditions that are present in other variables. It is a bit like `ifelse()`, but more powerful and flexible.

R users in psychology might use it to [recode variables](http://jenrichmond.rbind.io/post/recoding-variables/) or detect outliers.

In this example, we will use it to identify very large and very small penguins.

# show us

### install and load packages

You can access the `case_when()` function by installing and loading the tidyverse.

``` r
# install.packages("tidyverse")

library(tidyverse)
library(palmerpenguins)
library(gt)
```

### get some data

I am going to use the penguins data from the palmer penguins package to demo the `case_when()` function.

``` r
penguins <- penguins

glimpse(penguins)
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

### use the function

Lets imagine we were interested in identifying penguins that were extremely large or small. We can use `mutate()` and `case_when()` to make a new variable that puts different values in the new column depending on whether the body mass values are more (or less) than 2 standard deviations away from the mean. Because male penguins tend to be bigger than female penguins, we want to identify extreme penguins separately for males and females.

First, lets get some summary statistics including values of body mass that would be 2 standard deviations above and below the mean.

``` r
summary <- penguins %>%
  na.omit() %>% # remove penguins w missing values
    group_by(sex) %>%
  summarise(mean = mean(body_mass_g), 
            sd = sd(body_mass_g), 
            two_sd = sd*2, 
            twosd_above = mean + two_sd, 
            twosd_below = mean - two_sd) 

summary %>% 
   gt() %>% 
   fmt_number(
    columns = 2:6,
    decimals = 2,
    use_seps = FALSE
  )
```

<div id="zzwmjjcnku" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zzwmjjcnku .gt_table {
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

#zzwmjjcnku .gt_heading {
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

#zzwmjjcnku .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zzwmjjcnku .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zzwmjjcnku .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zzwmjjcnku .gt_col_headings {
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

#zzwmjjcnku .gt_col_heading {
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

#zzwmjjcnku .gt_column_spanner_outer {
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

#zzwmjjcnku .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zzwmjjcnku .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zzwmjjcnku .gt_column_spanner {
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

#zzwmjjcnku .gt_group_heading {
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

#zzwmjjcnku .gt_empty_group_heading {
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

#zzwmjjcnku .gt_from_md > :first-child {
  margin-top: 0;
}

#zzwmjjcnku .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zzwmjjcnku .gt_row {
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

#zzwmjjcnku .gt_stub {
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

#zzwmjjcnku .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zzwmjjcnku .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#zzwmjjcnku .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zzwmjjcnku .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zzwmjjcnku .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zzwmjjcnku .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zzwmjjcnku .gt_footnotes {
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

#zzwmjjcnku .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#zzwmjjcnku .gt_sourcenotes {
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

#zzwmjjcnku .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#zzwmjjcnku .gt_left {
  text-align: left;
}

#zzwmjjcnku .gt_center {
  text-align: center;
}

#zzwmjjcnku .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zzwmjjcnku .gt_font_normal {
  font-weight: normal;
}

#zzwmjjcnku .gt_font_bold {
  font-weight: bold;
}

#zzwmjjcnku .gt_font_italic {
  font-style: italic;
}

#zzwmjjcnku .gt_super {
  font-size: 65%;
}

#zzwmjjcnku .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">sex</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">sd</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">two_sd</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">twosd_above</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">twosd_below</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_center">female</td>
<td class="gt_row gt_right">3862.27</td>
<td class="gt_row gt_right">666.17</td>
<td class="gt_row gt_right">1332.34</td>
<td class="gt_row gt_right">5194.62</td>
<td class="gt_row gt_right">2529.93</td></tr>
    <tr><td class="gt_row gt_center">male</td>
<td class="gt_row gt_right">4545.68</td>
<td class="gt_row gt_right">787.63</td>
<td class="gt_row gt_right">1575.26</td>
<td class="gt_row gt_right">6120.94</td>
<td class="gt_row gt_right">2970.43</td></tr>
  </tbody>
  
  
</table>
</div>

Now we can use `case_when()` to mutate a new column called extremes. We want to code for penguins that are more (and less) than 2SD away from the mean body mass for their group. We can refer to values in our summary dataframe using indexing.

### how to read this code

``` r
penguins <- penguins %>%
  mutate(extremes = case_when(sex == "female" & body_mass_g < summary$twosd_below[1] ~ "small_girl", 
                              sex == "female" & body_mass_g > summary$twosd_above[1] ~ "big_girl", 
                               sex == "male" & body_mass_g < summary$twosd_below[2] ~ "small_boy", 
                               sex == "male" & body_mass_g > summary$twosd_above[2] ~ "big_boy",  
                              is.na(body_mass_g) ~ "no_bm",
                              is.na(sex) ~ "no_x",
                              TRUE ~ "not_extreme"))
```

-   we are making a new column called extremes that will populate based on cases in the sex column and body mass_g column
-   the first case is for female penguins who are little (i.e. less than 2SD below mean body mass), there are two conditions
    -   first sex == “female”
    -   second body_mass_g \< `summary$twosd_below[1]`
        -   I find it easiest to read the indexed values backwards…
        -   `summary$twosd_below[1]` refers to the
            -   1st row
            -   from the twosd_below column
            -   from the summary dataframe
-   the tilde \~ tells R what value you want in that cell aka “small_girl”
-   you can also mix up the kinds of conditions you specify, here it is going to put “no_bm” and “no_x” in the cases where we are missing body_mass_g and sex values
-   at the bottom of all the cases, include TRUE \~ “not extreme” to tell R what to do when the conditions above don’t apply; without this it will put NA by default

> Tip: `case_when()` will evaluate the statements from top to bottom, so sometimes you will need to think about the order that you write them. It is a good idea to order them from general to specific.

``` r
glimpse(penguins) # check the mutate worked
```

    ## Rows: 344
    ## Columns: 9
    ## $ species           <fct> Adelie, Adelie, Adelie, Adelie, Adelie, Adelie, Adel…
    ## $ island            <fct> Torgersen, Torgersen, Torgersen, Torgersen, Torgerse…
    ## $ bill_length_mm    <dbl> 39.1, 39.5, 40.3, NA, 36.7, 39.3, 38.9, 39.2, 34.1, …
    ## $ bill_depth_mm     <dbl> 18.7, 17.4, 18.0, NA, 19.3, 20.6, 17.8, 19.6, 18.1, …
    ## $ flipper_length_mm <int> 181, 186, 195, NA, 193, 190, 181, 195, 193, 190, 186…
    ## $ body_mass_g       <int> 3750, 3800, 3250, NA, 3450, 3650, 3625, 4675, 3475, …
    ## $ sex               <fct> male, female, female, NA, female, male, female, male…
    ## $ year              <int> 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007, 2007…
    ## $ extremes          <chr> "not_extreme", "not_extreme", "not_extreme", "no_bm"…

Now that we have a new column, we can `count()` how many penguins fall into the extreme categories or use our new variable to colour points on a `geom_jitter`

``` r
penguins %>%
  count(extremes)
```

    ## # A tibble: 5 × 2
    ##   extremes        n
    ##   <chr>       <int>
    ## 1 big_boy         1
    ## 2 big_girl        2
    ## 3 no_bm           2
    ## 4 no_x            9
    ## 5 not_extreme   330

``` r
penguins %>%
  filter(extremes != "no_bm") %>% # filter out penguins missing body mass data
  filter(extremes != "no_x") %>%  # filter out penguins missing sex data
  ggplot(aes(x = sex, y = body_mass_g, colour = extremes)) +
  geom_jitter(width = 0.2) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

# more resources…

-   functions in the `dplyr` package are really well documented; the [package vignette](https://dplyr.tidyverse.org/reference/case_when.html) in best place to find good examples.
-   I found the popups trying to sell me datascience courses on this site annoying but the content of [this blog post](https://www.sharpsightlabs.com/blog/case-when-r/) was useful.
-   Suzan Baert’s [blog posts](https://suzan.rbind.io/2018/02/dplyr-tutorial-2/) re all kinds of `dplyr` things are also good value.
-   … and Allison Horst has great art re `case_when()` and other functions on [her github](https://github.com/allisonhorst/stats-illustrations)

![](https://github.com/allisonhorst/stats-illustrations/raw/master/rstats-artwork/dplyr_case_when.png)<!-- -->
