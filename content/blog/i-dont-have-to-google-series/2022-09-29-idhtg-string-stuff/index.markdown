---
title: how to subset strings
author: Jen Richmond
date: '2022-09-29'
slug: []
categories: []
tags: []
---


Sometimes you have a column in your data frame that is text, but there is some of it that you don't need. Lets say your data looks like this...





```r
df <- data.frame(animals = c("this is a bear", "this is a lion", "this is a tiger"))

df
```

```
##           animals
## 1  this is a bear
## 2  this is a lion
## 3 this is a tiger
```

And perhaps only want the animal names... you can use `sub_str()` from the `stringr` package to strip out the extra characters. The `sub_str()` function allows you to specify the position of the character you want to start and end with. 

Here I want to start at the 11th character and keep the rest. 

> Note: spaces are included in your character count. 


```r
df_new <- df %>%
  mutate(new_col = str_sub(animals, start = 11))

df_new
```

```
##           animals new_col
## 1  this is a bear    bear
## 2  this is a lion    lion
## 3 this is a tiger   tiger
```
If you wanted to get just the "is a" piece of the string, you can specify both a start and end character. 

```r
df_new <- df_new %>%
  mutate(new_col2 = str_sub(animals, start = 6, end = 9))

df_new
```

```
##           animals new_col new_col2
## 1  this is a bear    bear     is a
## 2  this is a lion    lion     is a
## 3 this is a tiger   tiger     is a
```
You can also use - to count backwards from the end. Here I am selecting the last 9 characters. 


```r
df_new <- df_new %>%
  mutate(new_col3 = str_sub(animals, start = -9))

df_new
```

```
##           animals new_col new_col2  new_col3
## 1  this is a bear    bear     is a is a bear
## 2  this is a lion    lion     is a is a lion
## 3 this is a tiger   tiger     is a s a tiger
```

Yay- I no longer have to google (IDHTG) how to subset strings with stringr!
