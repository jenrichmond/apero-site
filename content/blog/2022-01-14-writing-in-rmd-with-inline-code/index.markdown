---
title: writing in Rmd with inline code
author: ''
date: '2022-01-14'
slug: []
categories:
  - Rmd
  - writing
  - reproducibility
tags:
  - Rmd
  - writing
  - reproducibility

---

One of the best things about RMarkdown is that you can use inline code to report summary and inferential statistics in your text. This means that it is impossible to make an error and if your data/values change, the text automatically updates.

Here I play with some penguin data and reporting summary stats using inline code.

# load packages/data

``` r
library(tidyverse)
library(janitor)
library(palmerpenguins)
library(gt)

options(digits=2)

penguins <- penguins
```

# count the penguins

Lets make a table that counts how many penguins there are in each species.

Here I’m using the `tabyl()` function from the `janitor` package to count how many penguins there are in each species and adorn a total column, then printing the table using `gt()`.

``` r
count_penguins <- penguins %>%
  tabyl(species) %>%
  adorn_totals() 


gt(count_penguins)
```

<div id="qezpshpaho" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#qezpshpaho .gt_table {
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

#qezpshpaho .gt_heading {
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

#qezpshpaho .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#qezpshpaho .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#qezpshpaho .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#qezpshpaho .gt_col_headings {
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

#qezpshpaho .gt_col_heading {
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

#qezpshpaho .gt_column_spanner_outer {
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

#qezpshpaho .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#qezpshpaho .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#qezpshpaho .gt_column_spanner {
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

#qezpshpaho .gt_group_heading {
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

#qezpshpaho .gt_empty_group_heading {
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

#qezpshpaho .gt_from_md > :first-child {
  margin-top: 0;
}

#qezpshpaho .gt_from_md > :last-child {
  margin-bottom: 0;
}

#qezpshpaho .gt_row {
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

#qezpshpaho .gt_stub {
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

#qezpshpaho .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#qezpshpaho .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#qezpshpaho .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#qezpshpaho .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#qezpshpaho .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#qezpshpaho .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#qezpshpaho .gt_footnotes {
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

#qezpshpaho .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#qezpshpaho .gt_sourcenotes {
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

#qezpshpaho .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#qezpshpaho .gt_left {
  text-align: left;
}

#qezpshpaho .gt_center {
  text-align: center;
}

#qezpshpaho .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#qezpshpaho .gt_font_normal {
  font-weight: normal;
}

#qezpshpaho .gt_font_bold {
  font-weight: bold;
}

#qezpshpaho .gt_font_italic {
  font-style: italic;
}

#qezpshpaho .gt_super {
  font-size: 65%;
}

#qezpshpaho .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">species</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">n</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">percent</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_left">Adelie</td>
<td class="gt_row gt_right">152</td>
<td class="gt_row gt_right">0.44</td></tr>
    <tr><td class="gt_row gt_left">Chinstrap</td>
<td class="gt_row gt_right">68</td>
<td class="gt_row gt_right">0.20</td></tr>
    <tr><td class="gt_row gt_left">Gentoo</td>
<td class="gt_row gt_right">124</td>
<td class="gt_row gt_right">0.36</td></tr>
    <tr><td class="gt_row gt_left">Total</td>
<td class="gt_row gt_right">344</td>
<td class="gt_row gt_right">1.00</td></tr>
  </tbody>
  
  
</table>
</div>

Now I can use inline text to refer to values in the count_penguins dataframe. The syntax goes like this…

> r dataframe\$column\[rownumber\]

For example, the following text in my Rmd file…

<img src="/Users/jennyrichmond/Documents/GitHub/apero-site/content/blog/2022-01-14-writing-in-rmd-with-inline-code/count_inline.png" width="741" />

… knits into the text below.

In the Palmer penguins dataset, there are body measurements from a total of 344 penguins. There are 3 species represented (N = 124 Gentoo, N = 68 Chinstrap and N = 152 Adelie).

Lets get some summary statistics.

``` r
body_mass <- penguins %>%
  group_by(species) %>%
  summarise(mean = mean(body_mass_g, na.rm = TRUE))

gt(body_mass)
```

<div id="ptutpsnktu" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;">
<style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#ptutpsnktu .gt_table {
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

#ptutpsnktu .gt_heading {
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

#ptutpsnktu .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#ptutpsnktu .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 6px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#ptutpsnktu .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ptutpsnktu .gt_col_headings {
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

#ptutpsnktu .gt_col_heading {
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

#ptutpsnktu .gt_column_spanner_outer {
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

#ptutpsnktu .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#ptutpsnktu .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#ptutpsnktu .gt_column_spanner {
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

#ptutpsnktu .gt_group_heading {
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

#ptutpsnktu .gt_empty_group_heading {
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

#ptutpsnktu .gt_from_md > :first-child {
  margin-top: 0;
}

#ptutpsnktu .gt_from_md > :last-child {
  margin-bottom: 0;
}

#ptutpsnktu .gt_row {
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

#ptutpsnktu .gt_stub {
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

#ptutpsnktu .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ptutpsnktu .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#ptutpsnktu .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#ptutpsnktu .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#ptutpsnktu .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#ptutpsnktu .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#ptutpsnktu .gt_footnotes {
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

#ptutpsnktu .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#ptutpsnktu .gt_sourcenotes {
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

#ptutpsnktu .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#ptutpsnktu .gt_left {
  text-align: left;
}

#ptutpsnktu .gt_center {
  text-align: center;
}

#ptutpsnktu .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#ptutpsnktu .gt_font_normal {
  font-weight: normal;
}

#ptutpsnktu .gt_font_bold {
  font-weight: bold;
}

#ptutpsnktu .gt_font_italic {
  font-style: italic;
}

#ptutpsnktu .gt_super {
  font-size: 65%;
}

#ptutpsnktu .gt_footnote_marks {
  font-style: italic;
  font-weight: normal;
  font-size: 65%;
}
</style>
<table class="gt_table">
  
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_center" rowspan="1" colspan="1">species</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">mean</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr><td class="gt_row gt_center">Adelie</td>
<td class="gt_row gt_right">3701</td></tr>
    <tr><td class="gt_row gt_center">Chinstrap</td>
<td class="gt_row gt_right">3733</td></tr>
    <tr><td class="gt_row gt_center">Gentoo</td>
<td class="gt_row gt_right">5076</td></tr>
  </tbody>
  
  
</table>
</div>

Now this text in my Rmd file…

<img src="/Users/jennyrichmond/Documents/GitHub/apero-site/content/blog/2022-01-14-writing-in-rmd-with-inline-code/mean.png" width="840" />

… knits into the text below.

On average, Gentoo penguins are the heaviest (*M* = 5076.02 g); Chinstrap (*M* = 3733.09 g) and Adelie (*M* = 3700.66 g) penguins are smaller.

# Reproducibility risks with inline code

Writing reports with Rmd can save you tons of time because once you have the code, you can reuse it with different data. But there are also risks… what if in the next penguin experiment, the mean body mass that ended up in the 3rd row of this table wasn’t for the Gentoo penguins, but rather some other species.

You can refer to rows by name in inline code using the column_to_rowname() function from `tibble`.

``` r
body_mass <- penguins %>%
  group_by(species) %>%
  summarise(mean = mean(body_mass_g, na.rm = TRUE)) %>%
  column_to_rownames(var = "species") # this replaces rownames that are numbers with species values

# print the rownames
rownames(body_mass)
```

    ## [1] "Adelie"    "Chinstrap" "Gentoo"

Once the species values are rownames, you can refer to a particular row,column by their name within square brackets. Not sure why you need quotes to refer by row/col name… its a mystery but it works!

<img src="/Users/jennyrichmond/Documents/GitHub/apero-site/content/blog/2022-01-14-writing-in-rmd-with-inline-code/rownames.png" width="837" />

On average, Gentoo penguins are the heaviest (*M* = 5076.02 g); Chinstrap (*M* = 3733.09 g) and Adelie (*M* = 3700.66 g) penguins are smaller.
