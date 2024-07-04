---
title: rowwise %>% mean
author: Package Build
date: '2023-05-24'
slug: []
categories: []
tags: []
---


When you have data from a survey, the responses for each item are most often listed in different variables. Generally you have to average across the items to get a mean value for that scale for each participant. But dealing with calculations across rows is sometimes difficult in R. 


### load packages + make some data


```r
library(tidyverse)


pID <- c("p1", "p2", "p3", "p4", "p5", "p6")
item1 = sample(1:7, 6, replace=T)
item2 = sample(1:7, 6, replace=T)
item3 = sample(1:7, 6, replace=T)
item4 = sample(1:7, 6, replace=T)
item5 = sample(1:7, 6, replace=T)

survey <- data.frame(pID, item1, item2, item3, item4, item5)

glimpse(survey)
```

```
## Rows: 6
## Columns: 6
## $ pID   <chr> "p1", "p2", "p3", "p4", "p5", "p6"
## $ item1 <int> 4, 1, 7, 5, 7, 1
## $ item2 <int> 2, 2, 4, 4, 7, 3
## $ item3 <int> 5, 2, 6, 7, 6, 3
## $ item4 <int> 6, 6, 2, 3, 1, 5
## $ item5 <int> 2, 2, 3, 4, 1, 4
```

### base R rowMeans

The rowMeans() function works, but why the x and what do the dots mean?? 


```r
survey_means_base <- survey %>%
  mutate(item_mean = rowMeans(x = select(.data = . , starts_with(match = "item"))))
```


### tidyverse rowwise 

The tidyverse version involves using rowwise() to tell R that you would like a mean calculated for each row in the dataset. Use c() to tell R which columns to average across. 

Without rowwise(), R will calculate the mean of all rows/columns and put that in the new variable. You will end up with the same value for each row. 


```r
survey_means_norowwise <- survey %>%
  mutate(item_mean = mean(c(item1, item2, item3, item4, item5))) 

glimpse(survey_means_norowwise)
```

```
## Rows: 6
## Columns: 7
## $ pID       <chr> "p1", "p2", "p3", "p4", "p5", "p6"
## $ item1     <int> 4, 1, 7, 5, 7, 1
## $ item2     <int> 2, 2, 4, 4, 7, 3
## $ item3     <int> 5, 2, 6, 7, 6, 3
## $ item4     <int> 6, 6, 2, 3, 1, 5
## $ item5     <int> 2, 2, 3, 4, 1, 4
## $ item_mean <dbl> 3.833333, 3.833333, 3.833333, 3.833333, 3.833333, 3.833333
```


With rowwise(), it calculates across the rows, separately for each participant. 

NOTE: it is important to get into the habit of adding ungroup() after a rowwise() in the same way as you would after a group_by() because the dataframe becomes grouped by row, which can mess with calcuations further down the pipeline. 


```r
survey_means_rowwise <- survey %>%
  rowwise() %>%
  mutate(item_mean = mean(c(item1, item2, item3, item4, item5))) %>%
  ungroup()

glimpse(survey_means_rowwise)
```

```
## Rows: 6
## Columns: 7
## $ pID       <chr> "p1", "p2", "p3", "p4", "p5", "p6"
## $ item1     <int> 4, 1, 7, 5, 7, 1
## $ item2     <int> 2, 2, 4, 4, 7, 3
## $ item3     <int> 5, 2, 6, 7, 6, 3
## $ item4     <int> 6, 6, 2, 3, 1, 5
## $ item5     <int> 2, 2, 3, 4, 1, 4
## $ item_mean <dbl> 3.8, 2.6, 4.4, 4.6, 4.4, 3.2
```

If there are a lot of columns to average across, you can avoid typing all of the names using c_across(). 


```r
survey_means_rowwise_across <- survey %>%
  rowwise() %>%
  mutate(item_mean = mean(c_across(item1:item5))) %>%
  ungroup()

glimpse(survey_means_rowwise_across)
```

```
## Rows: 6
## Columns: 7
## $ pID       <chr> "p1", "p2", "p3", "p4", "p5", "p6"
## $ item1     <int> 4, 1, 7, 5, 7, 1
## $ item2     <int> 2, 2, 4, 4, 7, 3
## $ item3     <int> 5, 2, 6, 7, 6, 3
## $ item4     <int> 6, 6, 2, 3, 1, 5
## $ item5     <int> 2, 2, 3, 4, 1, 4
## $ item_mean <dbl> 3.8, 2.6, 4.4, 4.6, 4.4, 3.2
```

