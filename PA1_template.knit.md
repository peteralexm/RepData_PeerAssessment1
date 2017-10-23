---
title: "Reproducible Research - Course Project 1"
output: html_document
---


This report contains analysis of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

1 Assignment
============

R code:

```r
library(ggplot2)

df <- read.csv("./activity.csv")

Sum_steps <- aggregate(cbind(steps)~date, data=df, FUN=sum)

plot1 <- ggplot(Sum_steps, aes(steps))+geom_histogram()+ 
  labs(title="Histogram for Steps") +  labs(x="Age", y="Count")+stat_bin(bins=30)+ylim(0,10)+
  geom_vline(xintercept=mean(Sum_steps$steps), color="red")+
  geom_vline(xintercept=median(Sum_steps$steps, na.rm=FALSE), color="blue")+
  scale_colour_manual(name="Line Color", values=c(myline1="red", myline2="blue", myline3="purple"))

median_steps <- median(Sum_steps$steps)
mean_steps <- mean(Sum_steps$steps)
```
A histogram of the total number of steps taken each day:

<img src="C:\Users\Ïåòÿ\Documents\datasciencecoursera\C5Project\RepData_PeerAssessment1\PA1_template_files/figure-html/unnamed-chunk-2-1.png" width="672" />
  
The mean of the total number of steps taken per day is 1.0766189\times 10^{4}.

The median of the total number of steps taken per day is 10765.

2 Assignment
============

R code:



```r
pattern <- aggregate(cbind(steps)~interval, data=df, FUN=mean)
plot2 <-ggplot(pattern, aes(x=interval, y=steps))+geom_line()

max_interval <- pattern$interval[which.max(pattern$steps)]
```

A time-series plot of the average number of steps taken during each of the 5-minute intervals throughout the day:
<img src="C:\Users\Ïåòÿ\Documents\datasciencecoursera\C5Project\RepData_PeerAssessment1\PA1_template_files/figure-html/unnamed-chunk-4-1.png" width="672" />

The maximum average number of steps is taken in the interval number 835

3 Assignment
============

R code:


```r
na_sum <- sum(is.na(df$steps))
df$steps[is.na(df$steps)] <-pattern$steps[pattern$interval %in% df$interval[is.na(df$steps)]] 
Sum_steps <- aggregate(cbind(steps)~date, data=df, FUN=sum)


plot3 <- ggplot(Sum_steps, aes(steps))+geom_histogram()+ 
  labs(title="Histogram for Steps") +  labs(x="Age", y="Count")+ylim(0,10)+
  geom_vline(xintercept=mean(Sum_steps$steps), color="red")+
  geom_vline(xintercept=median(Sum_steps$steps), color="blue")+
  scale_colour_manual(name="Line Color", values=c(myline1="red", myline2="blue", myline3="purple"))

median_steps_2 <- median(Sum_steps$steps)
mean_steps_2 <- mean(Sum_steps$steps)
```

The total number of missing values in the dataset is 2304.

The na values are replaced with the average values for the interval.

A histogram of the total number of steps taken each day after filling na values:

<img src="C:\Users\Ïåòÿ\Documents\datasciencecoursera\C5Project\RepData_PeerAssessment1\PA1_template_files/figure-html/unnamed-chunk-6-1.png" width="672" />
  
The mean of the total number of steps taken per day after filling na values is 1.0766189\times 10^{4}.

The median of the total number of steps taken per day after filling na values is 1.0766189\times 10^{4}.

4 Assignment
============

R Code:


```r
df$weekday <- weekdays(as.Date(df$date, format="%Y-%m-%d"), 1)
df$weekday[df$weekday=="Ïí"|df$weekday=="Âò"|df$weekday=="Ñð"|df$weekday=="×ò"|df$weekday=="Ïò"] <- "weekday"
df$weekday[df$weekday=="Ñá"|df$weekday=="Âñ"] <- "weekend"

pattern2 <- aggregate(cbind(steps)~interval+weekday, data=df, FUN=mean)

plot4 <- ggplot(pattern2, aes(x=interval, y=steps))+geom_line()+facet_grid(weekday ~.)
```

Plot showing the difference between workdays and weekends average steps pattern:
<img src="C:\Users\Ïåòÿ\Documents\datasciencecoursera\C5Project\RepData_PeerAssessment1\PA1_template_files/figure-html/unnamed-chunk-8-1.png" width="672" />
