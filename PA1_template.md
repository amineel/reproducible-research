Assignement week 2
==================

Loading the data
----------------

    activity<-read.csv("activity.csv")

-   transform date from factor to date

<!-- -->

    activity$date<-as.Date(as.character(activity$date))

Calculate the mean total number of steps taken per day
------------------------------------------------------

-   aggregate data using date

<!-- -->

    activityAggregated<-aggregate(steps~date,data=activity,FUN=sum,na.rm=TRUE)

-   Histogram of the total number of steps taken each day

<!-- -->

    hist(activityAggregated$steps)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-4-1.png)

-   Calculate and report the **mean** and **median** total number of
    steps taken per day

<!-- -->

    mean(activityAggregated$steps)

    ## [1] 10766.19

    median(activityAggregated$steps)

    ## [1] 10765

-   The **mean** total number of steps is 1.076618910^{4} steps.
-   The **median** total number of steps is 10765 steps.

Calculate the average daily activity pattern
--------------------------------------------

-   We are going to make a time series plot of the 5-minute interval and
    the average number of steps taken, averaged across all days.

<!-- -->

    activityDaily<-aggregate(steps~interval,data=activity,mean,na.rm=TRUE)
    library(ggplot2)

    ## Warning: package 'ggplot2' was built under R version 3.4.1

    qplot(steps,interval,data=activityDaily,geom ="line")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-6-1.png)

-   Which 5-minute interval, on average across all the days in the
    dataset, contains the maximum number of steps?

<!-- -->

    activityDaily[which.max(activityDaily$steps),]$interval

    ## [1] 835

The intervall is **835**

Imputing missing values
-----------------------

-   We are going to calculate total number of missing values in the
    dataset (i.e. the total number of rows with NAs)

<!-- -->

    sum(is.na(activity$steps))

    ## [1] 2304

Total of NA in the data is **2304**.

-   Now, we are going to fill missing data by using simply the mean of
    the day:

<!-- -->

    activityFilled<-activity
    activityFilled[is.na(activity$steps),]$steps<-mean(activityDaily$steps)

-   We are going now to plot an histogram of the total number of steps
    taken each day and Calculate and report the mean and median total
    number of steps taken per day.

<!-- -->

    totalStepsFilled<-aggregate(steps~date,data=activityFilled,sum)
    hist(totalStepsFilled$steps)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-10-1.png)

    mean(totalStepsFilled$steps)

    ## [1] 10766.19

    median(totalStepsFilled$steps)

    ## [1] 10766.19

-   The **mean** total number of steps taken per day is
    1.076618910^{4} steps.
-   The **median** total number of steps taken per day is
    1.076618910^{4} steps.

-   We notice that these values are the same before imputing data as we
    use the mean.

Are there differences in activity patterns between weekdays and weekends?
-------------------------------------------------------------------------

-   We will add a new factor into the data for week/weekends

<!-- -->

    weekdays <- c('lundi', 'mardi', 'mercredi', 'jeudi', 'vendredi')
    activityFilled$weekFator <- factor((weekdays(activityFilled$date) %in% weekdays),levels=c(FALSE, TRUE), labels=c('weekend', 'weekday'))

-   We will now make a panel plot containing a time series plot (i.e.
    type = "l") of the 5-minute interval and the average number of steps
    taken, averaged across all weekday days or weekend days

<!-- -->

    stepsIntervalWeekFactor=aggregate(steps~interval+weekFator,activityFilled,mean)
    qplot(y=steps,x=interval,data=stepsIntervalWeekFactor,geom ="line",facets =stepsIntervalWeekFactor$weekFator~.)

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-12-1.png)
