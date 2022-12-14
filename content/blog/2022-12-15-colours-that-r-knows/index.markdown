---
title: colours that R knows
author: Jen Richmond
date: '2022-12-15'
slug: []
categories: []
tags: []
---
 
I have been working through the [ggplot R Advent calendar by Kiirsti Owen](https://github.com/kiirsti/ggplot_adventcalendaR) with some lovely RLadies friends and we got up to Day 15 where we started controlling colours in ggplot with `scale_fill_manual()`. Our immediate question was "how to you know what the names of the colours that R knows are? 






```r
trees %>%
ggplot(aes(x=type, y=height))+
  geom_boxplot(aes(fill=type), colour="black")+
  theme_classic()+
  scale_fill_manual(values=c("darkgreen", "firebrick2", "mediumseagreen"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-2-1.png" width="672" />


A little googling revealed that you can have R list the names of all the colours it knows (there are 657 of them) using the `colours()` function, but that is not so useful if you want to see the difference between hotpink1 and hotpink2. 


We eventually found a function in the `epitools` package that will display all the colours and allow you to point a click the ones you want! It doesn't work so well in an Rmd chunk- you are best to try it in the console. 



```r
# install.packages("epitools")

library(epitools)

colors.plot(locator = TRUE)
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-3-1.png" width="672" />

```
## [1] color.names
## <0 rows> (or 0-length row.names)
```

Load the epitools package and then use the colors.plot() function in the console, setting locator = TRUE. A matrix will appear in your Plots tab. You can use your mouse to pick the colours you want and then click Finish to have R print the names of those colours to your console. 

<iframe width="560" height="315" src="https://www.youtube.com/embed/aMyi0m9ZD_k" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You can then use those names to revise your plot colours. 



```r
trees %>%
ggplot(aes(x=type, y=height))+
  geom_boxplot(aes(fill=type), colour="black")+
  theme_classic()+
  scale_fill_manual(values=c("seagreen", "maroon2", "dodgerblue2"))
```

<img src="{{< blogdown/postref >}}index_files/figure-html/unnamed-chunk-4-1.png" width="672" />
