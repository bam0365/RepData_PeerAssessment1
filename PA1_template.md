<h1>Loading and preprocessing the data</h1>


<code>
activityData<- read.csv("activity.csv")
</code>

<h1>What is mean total number of steps taken per day?</h1>

<code>
dailySteps <- aggregate(steps ~ date, activityData,sum)<br>
hist(dailySteps$steps, main=paste("Daily Total"), col="green", xlab="No of Steps")
</code>

<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot1.png" alt="Plot1" />
<br>

<code>
dailyMean <- mean(dailySteps$steps)<br>

dailyMedian <- median(dailySteps$steps)<br>
</code>

<h1>What is the average daily activity pattern?</h1>

stepsInterval <- aggregate(steps ~ interval, activityData,mean)<br>
plot(stepsInterval$interval, stepsInterval$steps,type="l", xlab="Interval",ylab="Steps")<br>
<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot2.png" alt="Plot2" />
<br>


<h1>Imputing missing values</h1>


incompleteData<- sum(!complete.cases(activityData))<br>
imputedData <- transform(activityData, steps=ifelse(is.na(activityData$steps),stepsInterval$steps[match(activityData$interval,stepsInterval$interval)],activityData$steps))<br>
imputedData[as.character(imputedData$date)=="2012-10-01",1] <-0<br>
stepsByDay <- aggregate(steps ~ date, imputedData, sum)<br>
hist(stepsByDay$steps, main=paste("Total Steps Each Day"), col="orange",xlab="Number of Steps")<br>
legend("topright", c("Imputed", "Non-imputed"), col=c("blue", "red"), lwd=10)<br>

<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot3.png" alt="Plot3" />
<br>

meanDelta <- rmeanImputed-dailyMean<br>
medianDelta <-rmedianImputed-dailyMedian<br>
totalDiff <- sum(stepsByDay$steps)-sum(dailySteps$steps)<br>

<h1>Are there differences in activity patterns between weekdays and weekends?</h1>


##Are there differences in activity patterns between weekdays and weekends?
weekdays <- c("Monday","Tuesday","Wednesday","Thursday","Friday")<br>
imputedData$DOW = as.factor(ifelse(is.element(weekdays(as.Date(imputedData$date)),weekdays),"Weekday","Weekend"))<br>
stepsByDay <-aggregate(steps ~ interval +DOW, imputedData,mean)<br>
library(lattice)<br>
xyplot(stepsByDay$steps ~ stepsByDay$interval |stepsByDay$DOW, meain="Average Steps per Day/Interval", xlab="Interval", ylab="Steps",layout=c(1,2),type="l")<br>

<img src="https://github.com/bam0365/RepData_PeerAssessment1/blob/master/plot4.png" alt="Plot4" />
<br>