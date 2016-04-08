
Reproducible Research: Peer Assessment 1
<h1>Loading and preprocessing the data</h1>

<h4>1. Load the data (i.e. read.csv())</code></h4>

<code>
activityDataRaw <- read.csv("activity.csv",stringsAsFactors=FALSE)
</code>

<h4>2. Process/transform the data (if necessary) into a format suitable for your analysis.</code></h4>
<code>
# Data formatting
<br>activityDataRaw$date <- as.POSIXct(activityDataRaw$date, format="%Y-%m-%d")

<br>activityDataRaw$interval <- as.factor(activityDataRaw$interval)

<br>activityDataRaw <- data.frame(date=activityDataRaw$date
                              ,weekday=tolower(weekdays(activityDataRaw$date))
                              ,steps=activityDataRaw$steps
                              ,interval=activityDataRaw$interval
                              )

<br>activityDataRaw <-cbind(activityDataRaw, daytype=ifelse(activityDataRaw$weekday=="saturday"|activityDataRaw$weekday=="sunday","weekend","weekday"))

<br># Clean Data

<br>activityData <- data.frame(date=activityDataRaw$date
                           ,weekday=activityDataRaw$weekday
                           ,daytype=activityDataRaw$daytype
                           ,interval=activityDataRaw$interval
                           ,steps=activityDataRaw$steps)

</code>

<hr>

<h1>What is mean total number of steps taken per day?</h1>

<h4>Make a histogram of the total number of steps taken each day?</h4>

<code>
<br>sumData <- aggregate(steps ~ date, activityData, sum)<br>
<br>names(sumData) <- c("data","steps")
<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot1.png" alt="lot1" />
<br><br>
<code>

<hr4>2. Calculate and report the mean and median total number of steps taken per day</hr>
<br>stepsMean<-mean(sumData$steps)
<br>stepsMedian<-median(sumData$steps)
<br>


<hr>

<h1>What is the average daily activity pattern?</h1>
<h4>1. Make a time series plot (i.e. type = “l”) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)</h4>
<br>meanData <- aggregate(activityData$steps
                     ,by=list(activityData$interval)
                     ,FUN=mean
                     ,na.rm=TRUE)

<br>names(meanData) <- c("interval","mean")
<br>
plot(meanData$interval
     ,meanData$mean
     ,type="l"
     ,col="blue"
     ,lwd=2
     ,xlab="Interval(Min)"
     ,ylab="Avg steps"
     ,main="Time series of avg steps per interval (excluding NA)")
<br>
<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot2.png" alt="plot2" />
<br>

<h4> 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?</h4>
<br>axPos <- which(meanData$mean == max(meanData$mean))
<br>maxInterval <- meanData[maxPos,1]
<br>which(meanData$mean == max(meanData$mean))

<br>
<hr>
<h1>Inputing missing values</h1>

<h4>1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)</h4>
<br>sum(is.na(activityData$steps))
<br>
<h4>Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.</h4>
<br>
naPos <- which(is.na(activityData$steps))
<br>meanVector <- rep(mean(activityData$steps, na.rm=TRUE), time=length(naPos))

<br><h4>3.Create a new dataset that is equal to the original dataset but with the missing data filled in.</h4>
<br>activityData[naPos,"steps"] <- meanVector
<br>summary(is.na(activityData$steps))
<br><h4>4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?</h4>
<br>sumData <- aggregate(activityData$steps, by=list(activityData$date),FUN=sum)
<br>names(sumData) <-c("Date","steps")
<br>ggplot(sumData,aes(x=steps)) + geom_histogram(fill="green", binwidth=1000)+ labs(title="Histogram of Step taken daily", x="Number of steps daily", y= "Count number of times daily")+ theme_bw()

<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot3.png" alt="plot3" />
<br>mean(sumData$Steps)
<br>median(sumData$Steps)


<h1>Are there differences ina ctivity patterns between weekdays and weekends?</h1>
<h4>1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.</h4>
<br>meanData<- aggregate(activityData$steps
                     ,by=list(activityData$daytype,activityData$weekday,activityData$interval),FUN=mean)
<br>names(meanData) <- c("DayType","Weekday", "Interval", "Mean")
<h4>2. Make a panel plot containing a time series plot (i.e. type = “l”) of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). The plot should look something like the following, which was created using simulated data:</h4>
<br>xyplot(Mean ~ Interval | DayType , meanData
       ,type="l"
       ,lwd="1"
       ,xlab="Interval"
       ,ylab="Number of steps"
       ,layout=c(1,2))
<br>

<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot3.png" alt="plot4" />

