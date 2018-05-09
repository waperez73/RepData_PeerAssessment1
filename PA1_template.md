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
library(ggplot2,quietly=TRUE)
```

## Loading and preprocessing the data

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

```r
#group by date to summarize the data
by_date<-group_by(data,date) 

by_date<-na.omit(by_date) #removing missing values 

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
table_avg_steps<-xtable(steps_per_day)
print(table_avg_steps,type="html")
```

<!-- html table generated in R 3.4.3 by xtable 1.8-2 package -->
<!-- Wed May 09 18:34:42 2018 -->
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

```r
ggplot(steps_per_day,aes(dates,average_steps))+geom_point(aes(date))
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

### 5. The 5-minute interval that, on average, contains the maximum number of steps

```r
ggplot(steps_per_day,aes(x=date,color=date))+geom_point(aes(y=total_steps))
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

```r
head(data[order(-data$steps),]) #top 6 steps per 5 minute intervals
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

## Imputing missing values

```r
warn.conflicts = FALSE
library(mice,quietly = TRUE)



## Are there differences in activity patterns between weekdays and weekends?
```
