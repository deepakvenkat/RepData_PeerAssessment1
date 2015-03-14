# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
The `dplyr` package is being used here to process the data better. 

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
## 
## The following object is masked from 'package:stats':
## 
##     filter
## 
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

The data is then read from the csv file. 

```r
activity = read.csv("activity.csv")
```


## What is mean total number of steps taken per day?
`dplyr` has a `group_by` function which can be used to find the total number 
of steps taken per day. The observations which don't have a steps are removed
for this. 


```r
steps_by_date <- activity %>%
  filter(!is.na(steps)) %>%
  group_by(date) %>%
  summarize(total_steps = sum(steps))
```

The `steps_by_date` variable produced in the previous step is then used to 
plot a histogram. 


```r
hist(steps_by_date$total_steps, col = 'red', xlab = "Total steps each day", 
     main = "Steps per day")
```

![](PA1_template_files/figure-html/histogram_steps-1.png) 

The mean and median are taken on the total_steps column

```r
mean(steps_by_date$total_steps)
```

```
## [1] 10766.19
```

```r
median(steps_by_date$total_steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?
Finding the average steps taken across all intervals :


```r
steps_by_interval <- activity %>%
  filter(!is.na(steps)) %>%
  group_by(interval) %>%
  summarize(avg_steps = mean(steps))
```

Plot a time series plot of the 5 minute interval and the average number of steps.



```r
plot(steps_by_interval$interval, steps_by_interval$avg_steps, type = "l",
     col = "blue", xlab = "Interval", ylab = "Avg Steps", 
     main = "Avg. steps taken per interval")
```

![](PA1_template_files/figure-html/interval_steps-1.png) 

Filtering by the interval which would have the maximum number of steps. 
This is then printed out to give the interval (along with the avg steps.)

```r
max_interval <- steps_by_interval %>%
  filter(avg_steps == max(avg_steps))
max_interval
```

```
## Source: local data frame [1 x 2]
## 
##   interval avg_steps
## 1      835  206.1698
```

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
