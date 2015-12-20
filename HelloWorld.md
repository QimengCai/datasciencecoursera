# HelloWorld.md
## This is a markdown file

---
title: 'Reproducible Research: Peer Assessment 1'
author: "QM"
date: "19 December, 2015"
output: html_document
---

# Loading and preprocessing the data
```{r}
setwd("/Users/MENGMENG/Desktop")
activity <- read.csv("activity.csv", colClasses = c(
            "numeric", "character", "numeric"))
# check the structure of the dateframe
str(activity)
# convert date into actual date format
activity$date <- as.Date(activity$date)
```
# What is mean total number of steps taken per day
```{r}
## calculate and plot the sum of daily steps
dailysteps <- tapply(activity$steps, activity$date, sum, na.rm=TRUE)
hist(dailysteps)
## calculate the mean and median total number of steps taken per day(NA removed)
mean <- sum(dailysteps) / length(dailysteps)
mean
median <- median(dailysteps)
median
```
# What is the average daily activity pattern?
```{r}
## calculate the average steps for every 5-minute interval(NA removed)
intervalavg <- aggregate(activity$steps,
                         by=list(activity$interval),
                         FUN=mean,
                         na.rm=TRUE)
names(intervalavg) <- c("interval", "mean")
```
```{r}
## plot 
plot(intervalavg, type="l", col="red", 
     xlab="5-minute interval",
     ylab="average steps",
     main="daily activity pattern")
```
```{r}
## find out the most active 5-mins interval
intervalavg[which.max(intervalavg$mean),]
```
# Imputing missing values
```{r}
## calculate and report the total number of missing values in the dataset
NAsum <- sum(is.na(activity))
NAsum
## fill in all the missing values with mean of 5-minute interval
for (i in which(sapply(activity, is.numeric))) {
        activity[is.na(activity[, i]), i] <- mean(activity[, i],  na.rm = TRUE)
}
## difference in daily pattern between original dataset and imputed dateset
newday <- tapply(activity$steps, activity$date, sum)
hist(newday)
mean(newday) - mean
median(newday) - median

```
# Are there differences in activity patterns between weekdays and weekends?
```{r}
## get the weekday 
datewd <- format(activity$date, "%A")
wd1 <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
activity$workday <- factor(datewd %in% wd1, levels=c(FALSE, TRUE), 
                  labels=c('weekend', 'weekday'))

meanSteps <- aggregate(activity$steps, list(as.numeric(activity$interval), activity$workday),
                       FUN = "mean")
names(meanSteps) <- c("interval","weekDays", "avgSteps")
```
```{r}
library(lattice)
xyplot(meanSteps$avgSteps ~ meanSteps$interval | meanSteps$weekDays, 
       layout = c(1, 2), type = "l", 
       xlab = "Interval", ylab = "Number of steps")

```
