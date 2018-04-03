---
title: "Peer_Assignment_1"
author: "Mustafa"
date: "March 31, 2018"
output:
  html_document:
    keep_md: true
---

# Loading and preprocessing the data

```r
  library(ggplot2) 
```

```
## Warning: package 'ggplot2' was built under R version 3.4.2
```

```r
  data <- read.csv("activity.csv")
```

# What is mean total number of steps taken per day?

```r
  spd <- aggregate(steps ~ date, data, sum, na.rm=TRUE)
```

## 1. Make a histogram of the total number of steps taken each day

```r
  barplot(spd$steps)   
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

## 2. Calculate and report the mean and median total number of steps taken per day

```r
  smean <- mean(spd$steps)
  smedian <- median(spd$steps)
```
* Mean is: 1.0766189\times 10^{4}
* Median is: 10765


# What is the average daily activity pattern?

```r
  avg <- tapply(data$steps, data$interval, mean, na.rm=TRUE)   
```

## 1. Make a time series plot

```r
   plot(names(avg), avg, xlab="5-min interval", type="l", ylab="the average number of steps")  
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

## 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
  maximum_avg <- max(avg)
  maximum_interval <- as.numeric(names(avg)[which(avg==max(avg))])
```
* Maximum steps: 206.1698113
* Maximum is at interval: 835

# Imputing missing values
## 1. Calculate and report the total number of missing values in the dataset

```r
   nas <- sum(is.na(data$steps))
```
* Total number of NAs is: 2304

## 2. Devise a strategy for filling in all of the missing values in the dataset

## 3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
  new_data <- data
  new_data$steps[which(is.na(data$steps))] <- as.vector(avg[as.character(data[which(is.na(data$steps)),3])])
```

## 4. Make a histogram of the total number of steps taken each day

```r
  spd2 <- aggregate(steps ~ date, new_data, sum, na.rm=TRUE)
  barplot(spd2$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

## and calculate and report the mean and median total number of steps taken per day.

```r
  smean2 <- mean(spd2$steps)
  smedian2 <- median(spd2$steps)
```

* New mean is: 1.0766189\times 10^{4}
* New median is: 1.0766189\times 10^{4}

# Are there differences in activity patterns between weekdays and weekends?

## 1. Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.'

```r
  new_data$dayType <- ifelse(as.POSIXlt(new_data$date)$wday %in% c(0,6), "weekends","weekdays")
  smedian2 <- median(spd2$steps)
```

## 2. Make a panel plot containing a time series plot

```r
  aggregateData <- aggregate(steps ~ interval + dayType, data=new_data, mean)
  ggplot(aggregateData, aes(interval, steps)) + geom_line() + facet_grid(dayType ~ .) + 
    xlab("5-minute interval") + ylab("avarage number of steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


