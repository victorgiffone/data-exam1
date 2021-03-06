---
title: 'Reproducible Research: Peer Assessment 1'
output:
  html_document:
    keep_md: yes
  pdf_document: default
---



begin with loading package


```r
library(readr)
library(ggplot2)
library(scales)
```

```
## 
## Attaching package: 'scales'
```

```
## The following object is masked from 'package:readr':
## 
##     col_factor
```

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```


Code for reading in the dataset and/or processing the data. 
BE CAREFUL, THIS IS WORKING WITH FRENCH DATE



```r
setwd("C:/Users/giffonev/Dropbox/TPR/reproducible-research-1")

activity <- read_csv("activity.csv", col_types = cols(steps = col_integer()))
activity$date <- as.Date(activity$date,format="%Y/%m/%d")
g <- ggplot(data=activity,aes(x=activity$date, y=activity$steps))
h <- ggplot(data=activity,aes(x=activity$interval, y=activity$steps))
activity$week <- 

     

activity$week <- sapply(activity$date,weekdays)
 for (i in 1:length(activity$steps)) {
    if (activity$week[i] == "samedi") {
         activity$week[i] <- "weekend"
    } else if (activity$week[i] == "dimanche") {
         activity$week[i] <- "weekend"
    } else {
         activity$week[i] <- "weekday"
    }
 }
activity$week <- as.factor(activity$week)
```

Histogram of the total number of steps taken each day


```r
g + geom_bar(stat = "identity") + labs(title = "Histogram of the total number of steps taken each day", x = "day", y = "total step per day") 
```

```
## Warning: Removed 2304 rows containing missing values (position_stack).
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->



Mean and median number of steps taken each day

```r
g + stat_summary(fun.y=mean, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "mean number of steps taken each day", x = "day", y = "mean step per day") + scale_x_date(labels = date_format("%m/%d"), breaks = "week") 
```

```
## Warning: Removed 2304 rows containing non-finite values (stat_summary).
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
g + stat_summary(fun.y=median, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "median number of steps taken each day", x = "day", y = "mean step per day") + scale_x_date(labels = date_format("%m/%d"), breaks = "week") 
```

```
## Warning: Removed 2304 rows containing non-finite values (stat_summary).
```

![](PA1_template_files/figure-html/unnamed-chunk-4-2.png)<!-- -->

Time series plot of the average number of steps taken

```r
h + stat_summary(fun.y=mean, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "Time series plot of the average number of steps taken", x = "time ", y = "mean step per time series") 
```

```
## Warning: Removed 2304 rows containing non-finite values (stat_summary).
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

The 5-minute interval that, on average, contains the maximum number of steps

```r
test <- tapply(activity$steps,activity$interval,mean, na.rm=TRUE)
max(test)
```

```
## [1] 206.1698
```

Code to describe and show a strategy for imputing missing data

```r
#number of NA : 
sum(is.na(activity$steps))
```

```
## [1] 2304
```

```r
#replacing NA with the mean : 
activity$steps[is.na(activity$steps)] <- mean(activity$steps, na.rm  = TRUE)
sum(is.na(activity$steps))
```

```
## [1] 0
```

Histogram of the total number of steps taken each day after missing values are imputed

```r
j <- ggplot(data=activity,aes(x=activity$date, y=activity$steps))
j + geom_bar(stat = "identity") + labs(title = "Histogram of the total number of steps taken each day after missing values are imputed", x = "day", y = "total step per day") 
```

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends


```r
k <- ggplot(data=activity,aes( x = activity$date, y=activity$steps, color = activity$week)) 
k + stat_summary(fun.y=mean, geom="line", size = 1) 
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

All of the R code needed to reproduce the results (numbers, plots, etc.) in the report



setwd("C:/Users/giffonev/Dropbox/TPR/reproducible-research-1")

activity <- read_csv("activity.csv", col_types = cols(steps = col_integer()))
activity$date <- as.Date(activity$date,format="%Y/%m/%d")
g <- ggplot(data=activity,aes(x=activity$date, y=activity$steps))
h <- ggplot(data=activity,aes(x=activity$interval, y=activity$steps))
activity$week <- 

     

activity$week <- sapply(activity$date,weekdays)
 for (i in 1:length(activity$steps)) {
    if (activity$week[i] == "samedi") {
         activity$week[i] <- "weekend"
    } else if (activity$week[i] == "dimanche") {
         activity$week[i] <- "weekend"
    } else {
         activity$week[i] <- "weekday"
    }
 }
activity$week <- as.factor(activity$week)


Histogram of the total number of steps taken each day



g + geom_bar(stat = "identity") + labs(title = "Histogram of the total number of steps taken each day", x = "day", y = "total step per day") 





Mean and median number of steps taken each day


g + stat_summary(fun.y=mean, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "mean number of steps taken each day", x = "day", y = "mean step per day") + scale_x_date(labels = date_format("%m/%d"), breaks = "week") 

g + stat_summary(fun.y=median, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "median number of steps taken each day", x = "day", y = "mean step per day") + scale_x_date(labels = date_format("%m/%d"), breaks = "week") 




Time series plot of the average number of steps taken

h + stat_summary(fun.y=mean, geom="point", shape=1, size = 1.5, col = "red") + labs(title = "Time series plot of the average number of steps taken", x = "time ", y = "mean step per time series") 


The 5-minute interval that, on average, contains the maximum number of steps


test <- tapply(activity$steps,activity$interval,mean, na.rm=TRUE)
max(test)


Code to describe and show a strategy for imputing missing data

#number of NA : 
sum(is.na(activity$steps))

#replacing NA with the mean : 
activity$steps[is.na(activity$steps)] <- mean(activity$steps, na.rm  = TRUE)
sum(is.na(activity$steps))


Histogram of the total number of steps taken each day after missing values are imputed

j <- ggplot(data=activity,aes(x=activity$date, y=activity$steps))
j + geom_bar(stat = "identity") + labs(title = "Histogram of the total number of steps taken each day after missing values are imputed", x = "day", y = "total step per day") 
 


Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends

k <- ggplot(data=activity,aes( x = activity$date, y=activity$steps, color = activity$week)) 
k + stat_summary(fun.y=mean, geom="line", size = 1) 





