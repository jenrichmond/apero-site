---
title: error bars on plots
author: Jen Richmond
date: '2023-04-25'
slug: []
categories: []
tags: []
---


Repurposing this APA figures post as a IDHTG post. 

As I write my first paper reporting data analysis coming out of R (woot!!!), here are some notes summarising all the googling I have done this morning about how to produce APA style figures in ggplot. 


__________________

### Load libraries
Start by loading `tidyverse` to get ggplot, `here` to make finding the data easy, and `papaja` to get the theme_apa() function. 

```r
library(tidyverse)
library(here)
library(papaja)
```

### Read in data

```r
plotdata <- read_csv("plotdata.csv")
```

```
## New names:
## Rows: 8 Columns: 9
## ── Column specification
## ──────────────────────────────────────────────────────── Delimiter: "," chr
## (4): direction, group, detailtype, groupnew dbl (5): ...1, mean, stdev, n,
## stderr
## ℹ Use `spec()` to retrieve the full column specification for this data. ℹ
## Specify the column types or set `show_col_types = FALSE` to quiet this message.
## • `` -> `...1`
```


```r
head(plotdata)
```

```
## # A tibble: 6 × 9
##    ...1 direction group     detailtype  mean stdev     n stderr groupnew       
##   <dbl> <chr>     <chr>     <chr>      <dbl> <dbl> <dbl>  <dbl> <chr>          
## 1     1 future    control   episodic    9.46  4.04    28  0.764 control group  
## 2     2 future    control   semantic    4.57  2.35    28  0.444 control group  
## 3     3 future    induction episodic    9.38  3.62    29  0.672 induction group
## 4     4 future    induction semantic    4.69  2.85    29  0.530 induction group
## 5     5 past      control   episodic   11.2   6.67    28  1.26  control group  
## 6     6 past      control   semantic    5.5   5.53    28  1.05  control group
```

# Basic ggplot (columns)
Plot separate bars for episodic vs semantic details, by past and future events, separately for kids in the control group vs. induction group. Get pairs of columns using position = "dodge". 


```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />

### Add error bars


```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew) + geom_errorbar(aes(ymin=mean-stderr, ymax=mean+stderr),
                  size=.3,    # Thinner lines
                    width=.2,
                      position=position_dodge(.9))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-5-1.png" width="672" />

### APA-ise
The theme_apa() from the pajaja package does most of the APAising. Gets rid of the grey and gridlines. But for some reason, now the bars are floating. 

```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew) + geom_errorbar(aes(ymin=mean-stderr, ymax=mean+stderr),
                  size=.3,    # Thinner lines
                    width=.2,
                      position=position_dodge(.9)) +
  theme_apa(base_size = 14)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-6-1.png" width="672" />

### Fix x and y axis
Extend y axis scale and make the bars sit on the x axis

```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew) + geom_errorbar(aes(ymin=mean-stderr, ymax=mean+stderr),
                  size=.3,    # Thinner lines
                    width=.2,
                      position=position_dodge(.9)) +
  theme_apa(base_size = 14) +
  scale_y_continuous(expand = c(0, 0), limits = c(0, 15)) # expand 0,0 to make the bars sit down
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-7-1.png" width="672" />

### Fix axis labels

Use the `\n` notation to break a label or title across two lines


```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew) + geom_errorbar(aes(ymin=mean-stderr, ymax=mean+stderr),
                  size=.3,    # Thinner lines
                    width=.2,
                      position=position_dodge(.9)) +
  theme_apa(base_size = 14) +
  scale_y_continuous(expand = c(0, 0), limits = c(0, 15)) +
   labs(x="Detail type", y="Mean number of details \n produced")
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-8-1.png" width="672" />

### Make grey scale

Use scale_fill_grey(), values 1 = white and 0 = black, specify values in between to get shades of grey 


```r
plotdata %>%
  ggplot(aes(x= detailtype, y = mean, fill = direction)) +
    geom_col(position = "dodge") +
  facet_wrap(~ groupnew) + geom_errorbar(aes(ymin=mean-stderr, ymax=mean+stderr),
                  size=.3,    # Thinner lines
                    width=.2,
                      position=position_dodge(.9)) +
  theme_apa(base_size = 14) +
  scale_y_continuous(expand = c(0, 0), limits = c(0, 15)) +
   labs(x="Detail type", y="Mean number of details \n produced") +
  scale_fill_grey(start = 0.40, end = 0.6) 
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-9-1.png" width="672" />

### Save as png to add to your paper

Use ggsave("nameoffile.png") to save the last plot as png. 


```r
ggsave("plotforpaper.png")
```

```
## Saving 7 x 5 in image
```

