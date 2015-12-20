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
