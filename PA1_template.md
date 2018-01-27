# Reproducible Research: Course project 1

## Loading and preprocessing the data


```r
activity_file <- unzip("activity.zip")
activity_data <- read.csv(activity_file)
activity_data_na <- activity_data
activity_data_na <- activity_data_na[complete.cases(activity_data_na),]
```

## Mean and Median of the total number of steps taken per day


```r
tsed <- aggregate(activity_data_na$steps, by=list(activity_data_na$date), FUN=sum)
plot(tsed$x, type = "h", lwd = 10, lend = "square", xlab = "Days", 
	ylab = "Number of steps") 
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)

```r
mean(activity_data_na$steps)
```

```
## [1] 37.3826
```

```r
median(activity_data_na$steps)
```

```
## [1] 0
```

## Average daily activity pattern


```r
asei <- aggregate(activity_data_na$steps, by=list(activity_data_na$interval), FUN=mean)
plot(asei$x, type = "l", xlab = "Interval", ylab = "Number of steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```r
max(activity_data_na$steps, na.rm = TRUE)
```

```
## [1] 806
```

## Imputing missing values


```r
missing <- is.na(activity_data$steps)
sum(missing)
```

```
## [1] 2304
```

```r
activity_data$steps[is.na(activity_data$steps)] <- mean(na.omit(activity_data$steps))
tsed <- aggregate(activity_data$steps, by=list(activity_data$date), FUN=sum)
plot(tsed$x, type = "h", lwd = 10, lend = "square", xlab = "Days", 
	ylab = "Number of steps") 
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)

```r
mean(activity_data$steps)
```

```
## [1] 37.3826
```

```r
median(activity_data$steps)
```

```
## [1] 0
```

## Activity patterns between weekdays and weekends

```r
activity_data$weekend <- weekdays(as.Date(activity_data$date)) == 'Saturday' |
				weekdays(as.Date(activity_data$date)) == 'Sunday'
weekday_data <- activity_data[activity_data$weekend == FALSE,]
weekend_data <- activity_data[activity_data$weekend == TRUE,]
plot(
	aggregate(weekday_data$steps, by=list(weekday_data$interval), FUN=mean), 
	type = "l", xlab = "Interval", ylab = "Number of steps", main = "Weekday"
)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)

```r
plot(
	aggregate(weekend_data$steps, by=list(weekend_data$interval), FUN=mean), 
	type = "l", xlab = "Interval", ylab = "Number of steps", main = "Weekend"
)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-2.png)
