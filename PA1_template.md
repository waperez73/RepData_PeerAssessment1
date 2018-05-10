---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

### Author: Wellintton Perez 2018

Loading some packages we will need later

```r
warn.conflicts = FALSE
library(dplyr,quietly=TRUE)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(ggplot2,quietly=TRUE)
```

```
## Warning: package 'ggplot2' was built under R version 3.4.4
```

## 1. Loading and preprocessing the data

```r
location=getwd() #use the current working directory

url<-"https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip"
filename<-"activity.csv"

data_location<-file.path(location,"data") # creates a repository to keep the raw data

data_file<-file.path(data_location,filename) #concat path and file name

#if the directory data does not existing download fresh data
if(!file.exists(data_file)){
  message("Download the activity data set.")
  rawdata<-tempfile()
  rawdata<-paste(rawdata,".zip")
  download.file(url,rawdata) #download data from the URL
  unzip(rawdata,exdir=data_location)
}

data=read.csv2(data_file,sep=",") #Reading the file

data$date=as.Date(data$date)  #need to convert to date
```
## What is mean total number of steps taken per day?

calculating the number of steps taken per day

```r
data_with_no_na<-na.omit(data) #removing missing values 

#group by date to summarize the data
by_date<-group_by(data_with_no_na,date) 

steps_per_day<-summarize(by_date,
                         obs=n(),
                         total_steps=sum(steps),
                         average_steps=mean(steps))
```
### Histogram of the total number of steps taken each day

```r
#histogram
ggplot(steps_per_day,aes(color=date))+geom_histogram(aes(total_steps))+
  ggtitle("Histogram of total steps per day")+
  xlab("Total steps")+
  ylab("Frequency")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

### This is a table containing the calculated mean and median steps per day

```r
library(xtable,quietly=TRUE)
```

```
## Warning: package 'xtable' was built under R version 3.4.4
```

```r
table_avg_steps<-xtable(steps_per_day)
print(table_avg_steps,type="html")
```

<!-- html table generated in R 3.4.3 by xtable 1.8-2 package -->
<!-- Wed May 09 21:57:50 2018 -->
<table border=1>
<tr> <th>  </th> <th> date </th> <th> obs </th> <th> total_steps </th> <th> average_steps </th>  </tr>
  <tr> <td align="right"> 1 </td> <td align="right"> 15615.00 </td> <td align="right"> 288 </td> <td align="right"> 126 </td> <td align="right"> 0.44 </td> </tr>
  <tr> <td align="right"> 2 </td> <td align="right"> 15616.00 </td> <td align="right"> 288 </td> <td align="right"> 11352 </td> <td align="right"> 39.42 </td> </tr>
  <tr> <td align="right"> 3 </td> <td align="right"> 15617.00 </td> <td align="right"> 288 </td> <td align="right"> 12116 </td> <td align="right"> 42.07 </td> </tr>
  <tr> <td align="right"> 4 </td> <td align="right"> 15618.00 </td> <td align="right"> 288 </td> <td align="right"> 13294 </td> <td align="right"> 46.16 </td> </tr>
  <tr> <td align="right"> 5 </td> <td align="right"> 15619.00 </td> <td align="right"> 288 </td> <td align="right"> 15420 </td> <td align="right"> 53.54 </td> </tr>
  <tr> <td align="right"> 6 </td> <td align="right"> 15620.00 </td> <td align="right"> 288 </td> <td align="right"> 11015 </td> <td align="right"> 38.25 </td> </tr>
  <tr> <td align="right"> 7 </td> <td align="right"> 15622.00 </td> <td align="right"> 288 </td> <td align="right"> 12811 </td> <td align="right"> 44.48 </td> </tr>
  <tr> <td align="right"> 8 </td> <td align="right"> 15623.00 </td> <td align="right"> 288 </td> <td align="right"> 9900 </td> <td align="right"> 34.38 </td> </tr>
  <tr> <td align="right"> 9 </td> <td align="right"> 15624.00 </td> <td align="right"> 288 </td> <td align="right"> 10304 </td> <td align="right"> 35.78 </td> </tr>
  <tr> <td align="right"> 10 </td> <td align="right"> 15625.00 </td> <td align="right"> 288 </td> <td align="right"> 17382 </td> <td align="right"> 60.35 </td> </tr>
  <tr> <td align="right"> 11 </td> <td align="right"> 15626.00 </td> <td align="right"> 288 </td> <td align="right"> 12426 </td> <td align="right"> 43.15 </td> </tr>
  <tr> <td align="right"> 12 </td> <td align="right"> 15627.00 </td> <td align="right"> 288 </td> <td align="right"> 15098 </td> <td align="right"> 52.42 </td> </tr>
  <tr> <td align="right"> 13 </td> <td align="right"> 15628.00 </td> <td align="right"> 288 </td> <td align="right"> 10139 </td> <td align="right"> 35.20 </td> </tr>
  <tr> <td align="right"> 14 </td> <td align="right"> 15629.00 </td> <td align="right"> 288 </td> <td align="right"> 15084 </td> <td align="right"> 52.38 </td> </tr>
  <tr> <td align="right"> 15 </td> <td align="right"> 15630.00 </td> <td align="right"> 288 </td> <td align="right"> 13452 </td> <td align="right"> 46.71 </td> </tr>
  <tr> <td align="right"> 16 </td> <td align="right"> 15631.00 </td> <td align="right"> 288 </td> <td align="right"> 10056 </td> <td align="right"> 34.92 </td> </tr>
  <tr> <td align="right"> 17 </td> <td align="right"> 15632.00 </td> <td align="right"> 288 </td> <td align="right"> 11829 </td> <td align="right"> 41.07 </td> </tr>
  <tr> <td align="right"> 18 </td> <td align="right"> 15633.00 </td> <td align="right"> 288 </td> <td align="right"> 10395 </td> <td align="right"> 36.09 </td> </tr>
  <tr> <td align="right"> 19 </td> <td align="right"> 15634.00 </td> <td align="right"> 288 </td> <td align="right"> 8821 </td> <td align="right"> 30.63 </td> </tr>
  <tr> <td align="right"> 20 </td> <td align="right"> 15635.00 </td> <td align="right"> 288 </td> <td align="right"> 13460 </td> <td align="right"> 46.74 </td> </tr>
  <tr> <td align="right"> 21 </td> <td align="right"> 15636.00 </td> <td align="right"> 288 </td> <td align="right"> 8918 </td> <td align="right"> 30.97 </td> </tr>
  <tr> <td align="right"> 22 </td> <td align="right"> 15637.00 </td> <td align="right"> 288 </td> <td align="right"> 8355 </td> <td align="right"> 29.01 </td> </tr>
  <tr> <td align="right"> 23 </td> <td align="right"> 15638.00 </td> <td align="right"> 288 </td> <td align="right"> 2492 </td> <td align="right"> 8.65 </td> </tr>
  <tr> <td align="right"> 24 </td> <td align="right"> 15639.00 </td> <td align="right"> 288 </td> <td align="right"> 6778 </td> <td align="right"> 23.53 </td> </tr>
  <tr> <td align="right"> 25 </td> <td align="right"> 15640.00 </td> <td align="right"> 288 </td> <td align="right"> 10119 </td> <td align="right"> 35.14 </td> </tr>
  <tr> <td align="right"> 26 </td> <td align="right"> 15641.00 </td> <td align="right"> 288 </td> <td align="right"> 11458 </td> <td align="right"> 39.78 </td> </tr>
  <tr> <td align="right"> 27 </td> <td align="right"> 15642.00 </td> <td align="right"> 288 </td> <td align="right"> 5018 </td> <td align="right"> 17.42 </td> </tr>
  <tr> <td align="right"> 28 </td> <td align="right"> 15643.00 </td> <td align="right"> 288 </td> <td align="right"> 9819 </td> <td align="right"> 34.09 </td> </tr>
  <tr> <td align="right"> 29 </td> <td align="right"> 15644.00 </td> <td align="right"> 288 </td> <td align="right"> 15414 </td> <td align="right"> 53.52 </td> </tr>
  <tr> <td align="right"> 30 </td> <td align="right"> 15646.00 </td> <td align="right"> 288 </td> <td align="right"> 10600 </td> <td align="right"> 36.81 </td> </tr>
  <tr> <td align="right"> 31 </td> <td align="right"> 15647.00 </td> <td align="right"> 288 </td> <td align="right"> 10571 </td> <td align="right"> 36.70 </td> </tr>
  <tr> <td align="right"> 32 </td> <td align="right"> 15649.00 </td> <td align="right"> 288 </td> <td align="right"> 10439 </td> <td align="right"> 36.25 </td> </tr>
  <tr> <td align="right"> 33 </td> <td align="right"> 15650.00 </td> <td align="right"> 288 </td> <td align="right"> 8334 </td> <td align="right"> 28.94 </td> </tr>
  <tr> <td align="right"> 34 </td> <td align="right"> 15651.00 </td> <td align="right"> 288 </td> <td align="right"> 12883 </td> <td align="right"> 44.73 </td> </tr>
  <tr> <td align="right"> 35 </td> <td align="right"> 15652.00 </td> <td align="right"> 288 </td> <td align="right"> 3219 </td> <td align="right"> 11.18 </td> </tr>
  <tr> <td align="right"> 36 </td> <td align="right"> 15655.00 </td> <td align="right"> 288 </td> <td align="right"> 12608 </td> <td align="right"> 43.78 </td> </tr>
  <tr> <td align="right"> 37 </td> <td align="right"> 15656.00 </td> <td align="right"> 288 </td> <td align="right"> 10765 </td> <td align="right"> 37.38 </td> </tr>
  <tr> <td align="right"> 38 </td> <td align="right"> 15657.00 </td> <td align="right"> 288 </td> <td align="right"> 7336 </td> <td align="right"> 25.47 </td> </tr>
  <tr> <td align="right"> 39 </td> <td align="right"> 15659.00 </td> <td align="right"> 288 </td> <td align="right">  41 </td> <td align="right"> 0.14 </td> </tr>
  <tr> <td align="right"> 40 </td> <td align="right"> 15660.00 </td> <td align="right"> 288 </td> <td align="right"> 5441 </td> <td align="right"> 18.89 </td> </tr>
  <tr> <td align="right"> 41 </td> <td align="right"> 15661.00 </td> <td align="right"> 288 </td> <td align="right"> 14339 </td> <td align="right"> 49.79 </td> </tr>
  <tr> <td align="right"> 42 </td> <td align="right"> 15662.00 </td> <td align="right"> 288 </td> <td align="right"> 15110 </td> <td align="right"> 52.47 </td> </tr>
  <tr> <td align="right"> 43 </td> <td align="right"> 15663.00 </td> <td align="right"> 288 </td> <td align="right"> 8841 </td> <td align="right"> 30.70 </td> </tr>
  <tr> <td align="right"> 44 </td> <td align="right"> 15664.00 </td> <td align="right"> 288 </td> <td align="right"> 4472 </td> <td align="right"> 15.53 </td> </tr>
  <tr> <td align="right"> 45 </td> <td align="right"> 15665.00 </td> <td align="right"> 288 </td> <td align="right"> 12787 </td> <td align="right"> 44.40 </td> </tr>
  <tr> <td align="right"> 46 </td> <td align="right"> 15666.00 </td> <td align="right"> 288 </td> <td align="right"> 20427 </td> <td align="right"> 70.93 </td> </tr>
  <tr> <td align="right"> 47 </td> <td align="right"> 15667.00 </td> <td align="right"> 288 </td> <td align="right"> 21194 </td> <td align="right"> 73.59 </td> </tr>
  <tr> <td align="right"> 48 </td> <td align="right"> 15668.00 </td> <td align="right"> 288 </td> <td align="right"> 14478 </td> <td align="right"> 50.27 </td> </tr>
  <tr> <td align="right"> 49 </td> <td align="right"> 15669.00 </td> <td align="right"> 288 </td> <td align="right"> 11834 </td> <td align="right"> 41.09 </td> </tr>
  <tr> <td align="right"> 50 </td> <td align="right"> 15670.00 </td> <td align="right"> 288 </td> <td align="right"> 11162 </td> <td align="right"> 38.76 </td> </tr>
  <tr> <td align="right"> 51 </td> <td align="right"> 15671.00 </td> <td align="right"> 288 </td> <td align="right"> 13646 </td> <td align="right"> 47.38 </td> </tr>
  <tr> <td align="right"> 52 </td> <td align="right"> 15672.00 </td> <td align="right"> 288 </td> <td align="right"> 10183 </td> <td align="right"> 35.36 </td> </tr>
  <tr> <td align="right"> 53 </td> <td align="right"> 15673.00 </td> <td align="right"> 288 </td> <td align="right"> 7047 </td> <td align="right"> 24.47 </td> </tr>
   </table>

## What is the average daily activity pattern?
### 1. Time series plot showing the number of steps taken on average across all days

```r
average_steps_ts<-ts(steps_per_day$average_steps,frequency = 1)
plot.ts(average_steps_ts,xy.labels=c("Time series","Average steps per day"))
title("Average number of steps taken per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

### 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
highest<-data[order(-data$steps),]
head(highest) #top 6 steps per 5 minute intervals
```

```
##       steps       date interval
## 16492   806 2012-11-27      615
## 3277    802 2012-10-12      900
## 16487   794 2012-11-27      550
## 14201   789 2012-11-19      720
## 4136    786 2012-10-15      835
## 10194   785 2012-11-05      925
```
The 5-minute interval with the highest number of steps if interval 615 with 806 steps. 

## Imputing missing values
### 1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs) 

```r
temp_NA<-is.na(data)
missing_values<-length(data[temp_NA])
```
####The total number of missing values in the dataset is 2304.

### 2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

After careful analysis of the dataset I decided to set all missing values to zero.  The reason is that the complete day was missing values so I could not find a good method to impute the missing values.

### 3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
warn.conflicts = FALSE
data[is.na(data$steps),][[1]]<-0.001 #fill in missing values with zero

#group by date to summarize the data
by_date<-group_by(data_with_no_na,date) 

steps_per_day<-summarize(by_date,
                         obs=n(),
                         total_steps=sum(steps),
                         average_steps=mean(steps),
                         median_steps=median(steps))
```
### 4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day.

```r
#histogram
ggplot(steps_per_day,aes(color=date))+geom_histogram(aes(total_steps))+
  ggtitle("Histogram of total steps per day")+
  xlab("Total steps")+
  ylab("Frequency")
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

### This is a table containing the calculated mean and median steps per day after imputing the missing values

```r
library(xtable,quietly=TRUE)
table_avg_steps<-xtable(steps_per_day)
print(table_avg_steps,type="html")
```

<!-- html table generated in R 3.4.3 by xtable 1.8-2 package -->
<!-- Wed May 09 21:57:53 2018 -->
<table border=1>
<tr> <th>  </th> <th> date </th> <th> obs </th> <th> total_steps </th> <th> average_steps </th> <th> median_steps </th>  </tr>
  <tr> <td align="right"> 1 </td> <td align="right"> 15615.00 </td> <td align="right"> 288 </td> <td align="right"> 126 </td> <td align="right"> 0.44 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 2 </td> <td align="right"> 15616.00 </td> <td align="right"> 288 </td> <td align="right"> 11352 </td> <td align="right"> 39.42 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 3 </td> <td align="right"> 15617.00 </td> <td align="right"> 288 </td> <td align="right"> 12116 </td> <td align="right"> 42.07 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 4 </td> <td align="right"> 15618.00 </td> <td align="right"> 288 </td> <td align="right"> 13294 </td> <td align="right"> 46.16 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 5 </td> <td align="right"> 15619.00 </td> <td align="right"> 288 </td> <td align="right"> 15420 </td> <td align="right"> 53.54 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 6 </td> <td align="right"> 15620.00 </td> <td align="right"> 288 </td> <td align="right"> 11015 </td> <td align="right"> 38.25 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 7 </td> <td align="right"> 15622.00 </td> <td align="right"> 288 </td> <td align="right"> 12811 </td> <td align="right"> 44.48 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 8 </td> <td align="right"> 15623.00 </td> <td align="right"> 288 </td> <td align="right"> 9900 </td> <td align="right"> 34.38 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 9 </td> <td align="right"> 15624.00 </td> <td align="right"> 288 </td> <td align="right"> 10304 </td> <td align="right"> 35.78 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 10 </td> <td align="right"> 15625.00 </td> <td align="right"> 288 </td> <td align="right"> 17382 </td> <td align="right"> 60.35 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 11 </td> <td align="right"> 15626.00 </td> <td align="right"> 288 </td> <td align="right"> 12426 </td> <td align="right"> 43.15 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 12 </td> <td align="right"> 15627.00 </td> <td align="right"> 288 </td> <td align="right"> 15098 </td> <td align="right"> 52.42 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 13 </td> <td align="right"> 15628.00 </td> <td align="right"> 288 </td> <td align="right"> 10139 </td> <td align="right"> 35.20 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 14 </td> <td align="right"> 15629.00 </td> <td align="right"> 288 </td> <td align="right"> 15084 </td> <td align="right"> 52.38 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 15 </td> <td align="right"> 15630.00 </td> <td align="right"> 288 </td> <td align="right"> 13452 </td> <td align="right"> 46.71 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 16 </td> <td align="right"> 15631.00 </td> <td align="right"> 288 </td> <td align="right"> 10056 </td> <td align="right"> 34.92 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 17 </td> <td align="right"> 15632.00 </td> <td align="right"> 288 </td> <td align="right"> 11829 </td> <td align="right"> 41.07 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 18 </td> <td align="right"> 15633.00 </td> <td align="right"> 288 </td> <td align="right"> 10395 </td> <td align="right"> 36.09 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 19 </td> <td align="right"> 15634.00 </td> <td align="right"> 288 </td> <td align="right"> 8821 </td> <td align="right"> 30.63 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 20 </td> <td align="right"> 15635.00 </td> <td align="right"> 288 </td> <td align="right"> 13460 </td> <td align="right"> 46.74 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 21 </td> <td align="right"> 15636.00 </td> <td align="right"> 288 </td> <td align="right"> 8918 </td> <td align="right"> 30.97 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 22 </td> <td align="right"> 15637.00 </td> <td align="right"> 288 </td> <td align="right"> 8355 </td> <td align="right"> 29.01 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 23 </td> <td align="right"> 15638.00 </td> <td align="right"> 288 </td> <td align="right"> 2492 </td> <td align="right"> 8.65 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 24 </td> <td align="right"> 15639.00 </td> <td align="right"> 288 </td> <td align="right"> 6778 </td> <td align="right"> 23.53 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 25 </td> <td align="right"> 15640.00 </td> <td align="right"> 288 </td> <td align="right"> 10119 </td> <td align="right"> 35.14 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 26 </td> <td align="right"> 15641.00 </td> <td align="right"> 288 </td> <td align="right"> 11458 </td> <td align="right"> 39.78 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 27 </td> <td align="right"> 15642.00 </td> <td align="right"> 288 </td> <td align="right"> 5018 </td> <td align="right"> 17.42 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 28 </td> <td align="right"> 15643.00 </td> <td align="right"> 288 </td> <td align="right"> 9819 </td> <td align="right"> 34.09 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 29 </td> <td align="right"> 15644.00 </td> <td align="right"> 288 </td> <td align="right"> 15414 </td> <td align="right"> 53.52 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 30 </td> <td align="right"> 15646.00 </td> <td align="right"> 288 </td> <td align="right"> 10600 </td> <td align="right"> 36.81 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 31 </td> <td align="right"> 15647.00 </td> <td align="right"> 288 </td> <td align="right"> 10571 </td> <td align="right"> 36.70 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 32 </td> <td align="right"> 15649.00 </td> <td align="right"> 288 </td> <td align="right"> 10439 </td> <td align="right"> 36.25 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 33 </td> <td align="right"> 15650.00 </td> <td align="right"> 288 </td> <td align="right"> 8334 </td> <td align="right"> 28.94 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 34 </td> <td align="right"> 15651.00 </td> <td align="right"> 288 </td> <td align="right"> 12883 </td> <td align="right"> 44.73 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 35 </td> <td align="right"> 15652.00 </td> <td align="right"> 288 </td> <td align="right"> 3219 </td> <td align="right"> 11.18 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 36 </td> <td align="right"> 15655.00 </td> <td align="right"> 288 </td> <td align="right"> 12608 </td> <td align="right"> 43.78 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 37 </td> <td align="right"> 15656.00 </td> <td align="right"> 288 </td> <td align="right"> 10765 </td> <td align="right"> 37.38 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 38 </td> <td align="right"> 15657.00 </td> <td align="right"> 288 </td> <td align="right"> 7336 </td> <td align="right"> 25.47 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 39 </td> <td align="right"> 15659.00 </td> <td align="right"> 288 </td> <td align="right">  41 </td> <td align="right"> 0.14 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 40 </td> <td align="right"> 15660.00 </td> <td align="right"> 288 </td> <td align="right"> 5441 </td> <td align="right"> 18.89 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 41 </td> <td align="right"> 15661.00 </td> <td align="right"> 288 </td> <td align="right"> 14339 </td> <td align="right"> 49.79 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 42 </td> <td align="right"> 15662.00 </td> <td align="right"> 288 </td> <td align="right"> 15110 </td> <td align="right"> 52.47 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 43 </td> <td align="right"> 15663.00 </td> <td align="right"> 288 </td> <td align="right"> 8841 </td> <td align="right"> 30.70 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 44 </td> <td align="right"> 15664.00 </td> <td align="right"> 288 </td> <td align="right"> 4472 </td> <td align="right"> 15.53 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 45 </td> <td align="right"> 15665.00 </td> <td align="right"> 288 </td> <td align="right"> 12787 </td> <td align="right"> 44.40 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 46 </td> <td align="right"> 15666.00 </td> <td align="right"> 288 </td> <td align="right"> 20427 </td> <td align="right"> 70.93 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 47 </td> <td align="right"> 15667.00 </td> <td align="right"> 288 </td> <td align="right"> 21194 </td> <td align="right"> 73.59 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 48 </td> <td align="right"> 15668.00 </td> <td align="right"> 288 </td> <td align="right"> 14478 </td> <td align="right"> 50.27 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 49 </td> <td align="right"> 15669.00 </td> <td align="right"> 288 </td> <td align="right"> 11834 </td> <td align="right"> 41.09 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 50 </td> <td align="right"> 15670.00 </td> <td align="right"> 288 </td> <td align="right"> 11162 </td> <td align="right"> 38.76 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 51 </td> <td align="right"> 15671.00 </td> <td align="right"> 288 </td> <td align="right"> 13646 </td> <td align="right"> 47.38 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 52 </td> <td align="right"> 15672.00 </td> <td align="right"> 288 </td> <td align="right"> 10183 </td> <td align="right"> 35.36 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 53 </td> <td align="right"> 15673.00 </td> <td align="right"> 288 </td> <td align="right"> 7047 </td> <td align="right"> 24.47 </td> <td align="right"> 0.00 </td> </tr>
   </table>

#### Do these values differ from the estimates from the first part of the assignment? 
#### What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
warn.conflicts = FALSE
library(mice,quietly = TRUE)
```

```
## Warning: package 'mice' was built under R version 3.4.4
```

```
## Warning: package 'lattice' was built under R version 3.4.4
```

## Are there differences in activity patterns between weekdays and weekends?

