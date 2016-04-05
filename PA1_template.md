
#Loading and preprocessing the data


>activityData<- read.csv("activity.csv")
>What is mean total number of steps taken per day?
>dailySteps <- aggregate(steps ~ date, activityData,sum)
>hist(dailySteps$steps, main=paste("Daily Total"), col="green", xlab="No of Steps")
