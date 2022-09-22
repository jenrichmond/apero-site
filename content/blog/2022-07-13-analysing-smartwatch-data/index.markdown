---
title: analysing smartwatch data
author: Jen Richmond
date: '2022-07-13'
slug: []
categories: []
tags: []
---

Sometimes trying to replicate what someone is doing in a blogpost you find on twitter is a great way to learn something new. I am half heartedly thinking about trying to learn Python so when I saw [this post about analysing smartwatch data](https://thecleverprogrammer.com/2022/05/31/smartwatch-data-analysis-using-python/) on twitter I thought that it looked like interesting data and perhaps if I tried to do what they had done in R, that would be a useful way of starting to translate my R knowledge into python… maybe.

So here we go….

# load packages

``` r
library(tidyverse)
library(here)
library(naniar)
library(lubridate)
library(skimr)
library(ggeasy)
library(gt)
library(janitor)
```

# read in the data

``` r
df <- read_csv(here("content/blog/2022-07-13-analysing-smartwatch-data/dailyActivity_merged.csv"))
```

# look at the first few rows

``` r
head(df)
```

    ## # A tibble: 6 × 15
    ##        Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitie…
    ##     <dbl> <chr>             <dbl>         <dbl>           <dbl>            <dbl>
    ## 1  1.50e9 4/12/2016         13162          8.5             8.5                 0
    ## 2  1.50e9 4/13/2016         10735          6.97            6.97                0
    ## 3  1.50e9 4/14/2016         10460          6.74            6.74                0
    ## 4  1.50e9 4/15/2016          9762          6.28            6.28                0
    ## 5  1.50e9 4/16/2016         12669          8.16            8.16                0
    ## 6  1.50e9 4/17/2016          9705          6.48            6.48                0
    ## # … with 9 more variables: VeryActiveDistance <dbl>,
    ## #   ModeratelyActiveDistance <dbl>, LightActiveDistance <dbl>,
    ## #   SedentaryActiveDistance <dbl>, VeryActiveMinutes <dbl>,
    ## #   FairlyActiveMinutes <dbl>, LightlyActiveMinutes <dbl>,
    ## #   SedentaryMinutes <dbl>, Calories <dbl>

# check if there are NAs

A few ways to check NAs, the easiest uses `naniar` to visualise NAs with vis_miss()

``` r
vis_miss(df)
```

    ## Warning: `gather_()` was deprecated in tidyr 1.2.0.
    ## Please use `gather()` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

Alternatively you can use dplyr to summarise across the whole dataframe…

``` r
# using dplyr
df %>%
  summarise(missing = sum(is.na(.)))
```

    ## # A tibble: 1 × 1
    ##   missing
    ##     <int>
    ## 1       0

``` r
# or more simply w n_miss() from naniar
n_miss(df)
```

    ## [1] 0

… or separately for each variable

``` r
# using dplyr 
df %>%
  summarise_all(funs(sum(is.na(.))))
```

    ## Warning: `funs()` was deprecated in dplyr 0.8.0.
    ## Please use a list of either functions or lambdas: 
    ## 
    ##   # Simple named list: 
    ##   list(mean = mean, median = median)
    ## 
    ##   # Auto named with `tibble::lst()`: 
    ##   tibble::lst(mean, median)
    ## 
    ##   # Using lambdas
    ##   list(~ mean(., trim = .2), ~ median(., na.rm = TRUE))
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was generated.

    ## # A tibble: 1 × 15
    ##      Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitiesD…
    ##   <int>        <int>      <int>         <int>           <int>              <int>
    ## 1     0            0          0             0               0                  0
    ## # … with 9 more variables: VeryActiveDistance <int>,
    ## #   ModeratelyActiveDistance <int>, LightActiveDistance <int>,
    ## #   SedentaryActiveDistance <int>, VeryActiveMinutes <int>,
    ## #   FairlyActiveMinutes <int>, LightlyActiveMinutes <int>,
    ## #   SedentaryMinutes <int>, Calories <int>

``` r
# or more simply w miss_var_summary() from naniar

miss_var_summary(df)
```

    ## # A tibble: 15 × 3
    ##    variable                 n_miss pct_miss
    ##    <chr>                     <int>    <dbl>
    ##  1 Id                            0        0
    ##  2 ActivityDate                  0        0
    ##  3 TotalSteps                    0        0
    ##  4 TotalDistance                 0        0
    ##  5 TrackerDistance               0        0
    ##  6 LoggedActivitiesDistance      0        0
    ##  7 VeryActiveDistance            0        0
    ##  8 ModeratelyActiveDistance      0        0
    ##  9 LightActiveDistance           0        0
    ## 10 SedentaryActiveDistance       0        0
    ## 11 VeryActiveMinutes             0        0
    ## 12 FairlyActiveMinutes           0        0
    ## 13 LightlyActiveMinutes          0        0
    ## 14 SedentaryMinutes              0        0
    ## 15 Calories                      0        0

Take home message: there are no missing values in this dataset.

# look at data types

``` r
glimpse(df)
```

    ## Rows: 940
    ## Columns: 15
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityDate             <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/…
    ## $ TotalSteps               <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
    ## $ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveMinutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ FairlyActiveMinutes      <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ LightlyActiveMinutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ SedentaryMinutes         <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ Calories                 <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…

The ActivityDate variable is characters so we need to convert that to date format

``` r
df <- df %>%
  mutate(ActivityDate = mdy(ActivityDate))

class(df$ActivityDate)
```

    ## [1] "Date"

# make a new total minutes column

Lets mutate a new column that sums the activity minutes. We need to use rowwise here to let R know that we want to sum those values in each row.

``` r
df <- df %>%
  rowwise() %>%
  mutate(TotalMinutes = VeryActiveMinutes +FairlyActiveMinutes + LightlyActiveMinutes + SedentaryMinutes) %>%
  ungroup()  # remember to ungroup to make sure the next operation is not rowwise
```

# descriptives

``` r
options(scipen = 99) # avoid scientific notation

descriptives <- df %>%
  select(TotalSteps:TotalMinutes) %>%
  skim()

gt(descriptives)
```

<div id="shisejhjuk" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#shisejhjuk .gt_table {
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

#shisejhjuk .gt_heading {
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

#shisejhjuk .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#shisejhjuk .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#shisejhjuk .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#shisejhjuk .gt_col_headings {
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

#shisejhjuk .gt_col_heading {
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

#shisejhjuk .gt_column_spanner_outer {
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

#shisejhjuk .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#shisejhjuk .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#shisejhjuk .gt_column_spanner {
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

#shisejhjuk .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
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

#shisejhjuk .gt_empty_group_heading {
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

#shisejhjuk .gt_from_md > :first-child {
  margin-top: 0;
}

#shisejhjuk .gt_from_md > :last-child {
  margin-bottom: 0;
}

#shisejhjuk .gt_row {
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

#shisejhjuk .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#shisejhjuk .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#shisejhjuk .gt_row_group_first td {
  border-top-width: 2px;
}

#shisejhjuk .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#shisejhjuk .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#shisejhjuk .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#shisejhjuk .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#shisejhjuk .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#shisejhjuk .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#shisejhjuk .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#shisejhjuk .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#shisejhjuk .gt_footnotes {
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

#shisejhjuk .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#shisejhjuk .gt_sourcenotes {
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

#shisejhjuk .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#shisejhjuk .gt_left {
  text-align: left;
}

#shisejhjuk .gt_center {
  text-align: center;
}

#shisejhjuk .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#shisejhjuk .gt_font_normal {
  font-weight: normal;
}

#shisejhjuk .gt_font_bold {
  font-weight: bold;
}

#shisejhjuk .gt_font_italic {
  font-style: italic;
}

#shisejhjuk .gt_super {
  font-size: 65%;
}

#shisejhjuk .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#shisejhjuk .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#shisejhjuk .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#shisejhjuk .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#shisejhjuk .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#shisejhjuk .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">skim_type</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">skim_variable</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">n_missing</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">complete_rate</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.mean</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.sd</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.p0</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.p25</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.p50</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.p75</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.p100</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">numeric.hist</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">TotalSteps</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">7637.910638298</td>
<td class="gt_row gt_right">5087.150741753</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">3789.750</td>
<td class="gt_row gt_right">7405.500</td>
<td class="gt_row gt_right">10727.0000</td>
<td class="gt_row gt_right">36019.000000</td>
<td class="gt_row gt_left">▇▇▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">TotalDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">5.489702122</td>
<td class="gt_row gt_right">3.924605909</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">2.620</td>
<td class="gt_row gt_right">5.245</td>
<td class="gt_row gt_right">7.7125</td>
<td class="gt_row gt_right">28.030001</td>
<td class="gt_row gt_left">▇▆▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">TrackerDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">5.475351058</td>
<td class="gt_row gt_right">3.907275943</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">2.620</td>
<td class="gt_row gt_right">5.245</td>
<td class="gt_row gt_right">7.7100</td>
<td class="gt_row gt_right">28.030001</td>
<td class="gt_row gt_left">▇▆▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">LoggedActivitiesDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">0.108170940</td>
<td class="gt_row gt_right">0.619896518</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.0000</td>
<td class="gt_row gt_right">4.942142</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">VeryActiveDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">1.502680851</td>
<td class="gt_row gt_right">2.658941165</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.210</td>
<td class="gt_row gt_right">2.0525</td>
<td class="gt_row gt_right">21.920000</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">ModeratelyActiveDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">0.567542551</td>
<td class="gt_row gt_right">0.883580319</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.240</td>
<td class="gt_row gt_right">0.8000</td>
<td class="gt_row gt_right">6.480000</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">LightActiveDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">3.340819149</td>
<td class="gt_row gt_right">2.040655388</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1.945</td>
<td class="gt_row gt_right">3.365</td>
<td class="gt_row gt_right">4.7825</td>
<td class="gt_row gt_right">10.710000</td>
<td class="gt_row gt_left">▆▇▆▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">SedentaryActiveDistance</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">0.001606383</td>
<td class="gt_row gt_right">0.007346176</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">0.0000</td>
<td class="gt_row gt_right">0.110000</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">VeryActiveMinutes</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">21.164893617</td>
<td class="gt_row gt_right">32.844803057</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">4.000</td>
<td class="gt_row gt_right">32.0000</td>
<td class="gt_row gt_right">210.000000</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">FairlyActiveMinutes</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">13.564893617</td>
<td class="gt_row gt_right">19.987403954</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">0.000</td>
<td class="gt_row gt_right">6.000</td>
<td class="gt_row gt_right">19.0000</td>
<td class="gt_row gt_right">143.000000</td>
<td class="gt_row gt_left">▇▁▁▁▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">LightlyActiveMinutes</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">192.812765957</td>
<td class="gt_row gt_right">109.174699751</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">127.000</td>
<td class="gt_row gt_right">199.000</td>
<td class="gt_row gt_right">264.0000</td>
<td class="gt_row gt_right">518.000000</td>
<td class="gt_row gt_left">▅▇▇▃▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">SedentaryMinutes</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">991.210638298</td>
<td class="gt_row gt_right">301.267436790</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">729.750</td>
<td class="gt_row gt_right">1057.500</td>
<td class="gt_row gt_right">1229.5000</td>
<td class="gt_row gt_right">1440.000000</td>
<td class="gt_row gt_left">▁▁▇▅▇</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">Calories</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">2303.609574468</td>
<td class="gt_row gt_right">718.166862134</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1828.500</td>
<td class="gt_row gt_right">2134.000</td>
<td class="gt_row gt_right">2793.2500</td>
<td class="gt_row gt_right">4900.000000</td>
<td class="gt_row gt_left">▁▆▇▃▁</td></tr>
    <tr><td class="gt_row gt_left">numeric</td>
<td class="gt_row gt_left">TotalMinutes</td>
<td class="gt_row gt_right">0</td>
<td class="gt_row gt_right">1</td>
<td class="gt_row gt_right">1218.753191489</td>
<td class="gt_row gt_right">265.931767055</td>
<td class="gt_row gt_right">2</td>
<td class="gt_row gt_right">989.750</td>
<td class="gt_row gt_right">1440.000</td>
<td class="gt_row gt_right">1440.0000</td>
<td class="gt_row gt_right">1440.000000</td>
<td class="gt_row gt_left">▁▁▁▅▇</td></tr>
  </tbody>
  
  
</table>
</div>

# plot TotalSteps and calories burned

## the goal

![](https://i0.wp.com/thecleverprogrammer.com/wp-content/uploads/2022/05/smartwatch-1.png?w=1158&ssl=1)

In the python plot they use a “ols” trendline but I don’t really know what that is so using “lm” instead. The graph in the post has the size of the points plotting very active minutes but there isn’t a legend on the plot, so I am using a function from `ggeasy` to remove the legend. Also worked out how to make the y axis be labelled 0 - 40k, rather than 0-40000 using the labels argument in `scale_y_continuous`.

``` r
df %>%
  ggplot(aes(x = Calories, y = TotalSteps, size = VeryActiveMinutes)) +
  geom_point(colour = "blue", alpha = 0.5) + 
  geom_smooth(method = "lm", se = FALSE) +
  easy_remove_legend() +
  scale_y_continuous(limits = c(0,40000), labels = c("0", "10k", "20k", "30k", "40k"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-11-1.png" width="672" />

# pie chart

## the goal

![](https://i0.wp.com/thecleverprogrammer.com/wp-content/uploads/2022/05/smartwatch-2.png?w=1158&ssl=1)

The next graph in the blog post is a pie chart plotting the total active minutes in the 4 categories (inactive, lightly active, very active and fairly active). First I need to replicate these values. Luckily they are in the descriptives, so I am just going to select and filter everything else out of that dataframe.

``` r
tam <- descriptives %>%
  select(skim_variable, numeric.mean) %>%
  filter(skim_variable %in% c( "SedentaryMinutes", "LightlyActiveMinutes" , "FairlyActiveMinutes", "VeryActiveMinutes")) 

gt(tam)
```

<div id="zbxkqaibeh" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#zbxkqaibeh .gt_table {
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

#zbxkqaibeh .gt_heading {
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

#zbxkqaibeh .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#zbxkqaibeh .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#zbxkqaibeh .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zbxkqaibeh .gt_col_headings {
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

#zbxkqaibeh .gt_col_heading {
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

#zbxkqaibeh .gt_column_spanner_outer {
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

#zbxkqaibeh .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#zbxkqaibeh .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#zbxkqaibeh .gt_column_spanner {
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

#zbxkqaibeh .gt_group_heading {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
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

#zbxkqaibeh .gt_empty_group_heading {
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

#zbxkqaibeh .gt_from_md > :first-child {
  margin-top: 0;
}

#zbxkqaibeh .gt_from_md > :last-child {
  margin-bottom: 0;
}

#zbxkqaibeh .gt_row {
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

#zbxkqaibeh .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
}

#zbxkqaibeh .gt_stub_row_group {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 5px;
  padding-right: 5px;
  vertical-align: top;
}

#zbxkqaibeh .gt_row_group_first td {
  border-top-width: 2px;
}

#zbxkqaibeh .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zbxkqaibeh .gt_first_summary_row {
  border-top-style: solid;
  border-top-color: #D3D3D3;
}

#zbxkqaibeh .gt_first_summary_row.thick {
  border-top-width: 2px;
}

#zbxkqaibeh .gt_last_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zbxkqaibeh .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#zbxkqaibeh .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#zbxkqaibeh .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#zbxkqaibeh .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#zbxkqaibeh .gt_footnotes {
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

#zbxkqaibeh .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding-left: 4px;
  padding-right: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zbxkqaibeh .gt_sourcenotes {
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

#zbxkqaibeh .gt_sourcenote {
  font-size: 90%;
  padding-top: 4px;
  padding-bottom: 4px;
  padding-left: 5px;
  padding-right: 5px;
}

#zbxkqaibeh .gt_left {
  text-align: left;
}

#zbxkqaibeh .gt_center {
  text-align: center;
}

#zbxkqaibeh .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#zbxkqaibeh .gt_font_normal {
  font-weight: normal;
}

#zbxkqaibeh .gt_font_bold {
  font-weight: bold;
}

#zbxkqaibeh .gt_font_italic {
  font-style: italic;
}

#zbxkqaibeh .gt_super {
  font-size: 65%;
}

#zbxkqaibeh .gt_two_val_uncert {
  display: inline-block;
  line-height: 1em;
  text-align: right;
  font-size: 60%;
  vertical-align: -0.25em;
  margin-left: 0.1em;
}

#zbxkqaibeh .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 75%;
  vertical-align: 0.4em;
}

#zbxkqaibeh .gt_asterisk {
  font-size: 100%;
  vertical-align: 0;
}

#zbxkqaibeh .gt_slash_mark {
  font-size: 0.7em;
  line-height: 0.7em;
  vertical-align: 0.15em;
}

#zbxkqaibeh .gt_fraction_numerator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: 0.45em;
}

#zbxkqaibeh .gt_fraction_denominator {
  font-size: 0.6em;
  line-height: 0.6em;
  vertical-align: -0.05em;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">skim_variable</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">numeric.mean</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">VeryActiveMinutes</td>
<td class="gt_row gt_right">21.16489</td></tr>
    <tr><td class="gt_row gt_left">FairlyActiveMinutes</td>
<td class="gt_row gt_right">13.56489</td></tr>
    <tr><td class="gt_row gt_left">LightlyActiveMinutes</td>
<td class="gt_row gt_right">192.81277</td></tr>
    <tr><td class="gt_row gt_left">SedentaryMinutes</td>
<td class="gt_row gt_right">991.21064</td></tr>
  </tbody>
  
  
</table>
</div>

OK first thing to “fix” are the labels on these categories. Inactive seems like a better label than Sedentary. Make the skim variable a factor first. Then use `levels()` to check that there are now levels. Then use `fct_recode()` to change the labels on the factor levels manually.

``` r
glimpse(tam)
```

    ## Rows: 4
    ## Columns: 2
    ## $ skim_variable <chr> "VeryActiveMinutes", "FairlyActiveMinutes", "LightlyActi…
    ## $ numeric.mean  <dbl> 21.16489, 13.56489, 192.81277, 991.21064

``` r
tam <- tam %>%
  mutate(skim_variable = as_factor(skim_variable))

levels(tam$skim_variable)
```

    ## [1] "VeryActiveMinutes"    "FairlyActiveMinutes"  "LightlyActiveMinutes"
    ## [4] "SedentaryMinutes"

``` r
tam <- tam %>%
  mutate(skim_variable = fct_recode(skim_variable, 
                                    "Very Active Minutes" =  "VeryActiveMinutes", 
                                   "Fairly Active Minutes" = "FairlyActiveMinutes", 
                                   "Lightly Active Minutes" = "LightlyActiveMinutes", 
                                    "Inactive Minutes" = "SedentaryMinutes"))

levels(tam$skim_variable)
```

    ## [1] "Very Active Minutes"    "Fairly Active Minutes"  "Lightly Active Minutes"
    ## [4] "Inactive Minutes"

There isn’t a `geom_pie()` in ggplot, probably because pie charts are the *worst visualisation* but you can make one by first making a stacked bar chart using `geom_bar()` and then adding `coord_polar()`.

Good instructions available here https://r-graph-gallery.com/piechart-ggplot2.html

Bar graph version…

``` r
tam %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable)) +
  geom_bar(stat="identity") 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-14-1.png" width="672" />

… add `coord_polar()`

``` r
tam %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable)) +
  geom_bar(stat="identity", color="white") +
  coord_polar("y", start = 0) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-15-1.png" width="672" />

OK the bones are there but I really don’t want the axis labels or the grey background. Add `theme_void()` to get rid of those.

``` r
tam %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable)) +
  geom_bar(stat="identity", color="white") +
  coord_polar("y", start = 0) +
  theme_void()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-16-1.png" width="672" />

Awesome, now in the post they have the legend ordered by the mean (with Inactive at the top). I think you can do that within a mutate, right before your data hits ggplot \[see this post\] (https://r-graph-gallery.com/267-reorder-a-variable-in-ggplot2.html).

``` r
 tam %>%
  mutate(skim_variable = fct_reorder(skim_variable, desc(numeric.mean))) %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable)) +
  geom_bar(stat="identity", color="white") +
  coord_polar("y", start = 0) +
  theme_void()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-17-1.png" width="672" />

And they have ridiculous number labels… in the spirit of reproducibility, lets do that too!

``` r
tam %>%
  mutate(skim_variable = fct_reorder(skim_variable, desc(numeric.mean))) %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable, label = numeric.mean)) +
  geom_bar(stat="identity", color="white") +
  coord_polar("y", start = 0) +
  theme_void() +
  geom_text(angle = 45)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-18-1.png" width="672" />

Hmmmm I have overlapping numbers! I would be great to have more control over where the numbers go… I thought maybe `ggannotate` would help but it doesn’t work with polar coordinates. So I am stuck with position dodge. Adding a mutate to round the numbers also helps…

``` r
tam %>%
  mutate(skim_variable = fct_reorder(skim_variable, desc(numeric.mean))) %>%
  mutate(numeric.mean =  round(numeric.mean, 4)) %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable, label = numeric.mean)) +
  geom_bar(stat="identity", color="white") +
  coord_polar("y", start = 0) +
  theme_void() +
  geom_text(angle = 45, position = position_dodge(0.5))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-19-1.png" width="672" />

Not terrible, what about colours?? The original post has blue, pink, yellow and green.

-   blue 1 0 245 (#0100F5)
-   pink 246 194 203 (#F6C2CB)
-   yellow 249 217 73 (#F9D949)
-   green 166 236 153 (#A6EC99)

I worked out how to use the Digital Colour Meter from Utilities on my Mac ot get the exact RGB codes for the colours in the graph using [this resource](https://www.wikihow.com/Get-the-Hex-Code-of-a-Color-on-Your-Computer-Screen).

Then used this [RGB-Hex converter](https://www.rgbtohex.net/). I wonder if this step is necessary?? does ggplot know RGB codes??

Ahhh maybe not… but there is a `rgb` function, check this out!

``` r
rgb(1,0,245, maxColorValue = 255)
```

    ## [1] "#0100F5"

``` r
rgb(246,194,203, maxColorValue = 255)
```

    ## [1] "#F6C2CB"

``` r
rgb(249,217,73, maxColorValue = 255)
```

    ## [1] "#F9D949"

``` r
rgb(166,236,153, maxColorValue = 255)
```

    ## [1] "#A6EC99"

Adding in colours using `scale_fill_manual()` and removing the legend title with `ggeasy`.

``` r
tam %>%
  mutate(skim_variable = fct_reorder(skim_variable, desc(numeric.mean))) %>%
  mutate(numeric.mean =  round(numeric.mean, 4)) %>%
  ggplot(aes(x="", y=numeric.mean, fill=skim_variable, label = numeric.mean)) +
  geom_bar(stat="identity", color="white") +
  scale_fill_manual(values = c("#0100F5","#F6C2CB","#F9D949","#A6EC99")) +
  coord_polar("y", start = 0) +
  theme_void() +
  geom_text(angle = 45, position = position_dodge(0.5)) +
  easy_remove_legend_title()
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-21-1.png" width="672" />

Under the pie chart there are some summary stats…lets see if we can get those using inline code.

``` r
tam_wide <- tam %>%
  pivot_wider(names_from = skim_variable, values_from = numeric.mean) %>%
  clean_names() %>%
  rowwise() %>%
  mutate(total = very_active_minutes + fairly_active_minutes + lightly_active_minutes + inactive_minutes) %>%
  pivot_longer(names_to = "category", values_to = "minutes", very_active_minutes:inactive_minutes) %>%
  relocate(total, .after = minutes) %>%
  mutate(percent = (minutes/total)*100) %>%
  mutate(percent = round(percent, 1)) %>%
   mutate(minutes = round(minutes, 0))
```

**Observations**

1.  81.3 of Total inactive minutes in a day
2.  15.8 of Lightly active minutes in a day
3.  On an average, only 21 (1.7) were very active
4.  and 1.1 (14) of fairly active minutes in a day

# activity by day

## the goal

![](https://i0.wp.com/thecleverprogrammer.com/wp-content/uploads/2022/05/smartwatch-3.png?w=1158&ssl=1)

Next up there is a column plot that looks at activity by day of the week. The lubridate package makes it easy to pull the day out of a date. I am going back to the original data frame and making a new one that includes just the id and activity date and the activity in minutes.

``` r
glimpse(df)
```

    ## Rows: 940
    ## Columns: 16
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityDate             <date> 2016-04-12, 2016-04-13, 2016-04-14, 2016-04-…
    ## $ TotalSteps               <dbl> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
    ## $ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveMinutes        <dbl> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ FairlyActiveMinutes      <dbl> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ LightlyActiveMinutes     <dbl> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ SedentaryMinutes         <dbl> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ Calories                 <dbl> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…
    ## $ TotalMinutes             <dbl> 1094, 1033, 1440, 998, 1040, 761, 1440, 1120,…

``` r
day <- df %>%
  clean_names() %>%
  select(id:activity_date, very_active_minutes:calories) %>%
  mutate(day = wday(activity_date, label = TRUE)) %>%
  rename(inactive_minutes = sedentary_minutes) %>%
  pivot_longer(names_to = "category", values_to = "minutes", very_active_minutes:inactive_minutes) %>%
  mutate(category = str_sub(category, end = -9)) %>%
  mutate(category = fct_relevel(category, c("very_active", "fairly_active", "lightly_active", "inactive")))

day %>%
  filter(category != "inactive") %>%
  group_by(day, category) %>%
  summarise(activity = sum(minutes)) %>%
  ggplot(aes(x = day, y = activity, fill = category)) +
  geom_col(position = "dodge") +
   scale_fill_manual(values = c("purple", "darkgreen", "pink")) +
   scale_y_continuous(limits = c(0,40000), labels = c("0", "10k", "20k", "30k", "40k")) +
  easy_remove_legend_title()
```

    ## `summarise()` has grouped output by 'day'. You can override using the `.groups`
    ## argument.

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-23-1.png" width="672" />
