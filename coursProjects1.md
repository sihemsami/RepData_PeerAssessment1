---
title: "Cours Project1"
output:
  html_document: default
  pdf_document: default
  word_document: default
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

## R Markdown

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

## 1 Code for reading in the dataset and/or processing the data

```{r }
AMD<-read.csv("activity.csv",header=TRUE)
```
## 2 Histogram of the total number of steps taken each day
```{r}
AMDsum<-aggregate(.~date,data=AMD[,1:2],FUN=sum,na.action = na.pass)
AMDsum[is.na(AMDsum)]<-0
hist(AMDsum[,2],breaks = 10,xlab="Number of steps",main = "Total number of steps taken each day",col="lightBlue")
```

## 3 Mean and median number of steps taken each day

```{r}
AMDmean<-mean(AMDsum[,2])
```
```{r AMDmean,echo=FALSE}
AMDmean
```
```{r}
AMDmedian<-mean(AMDsum[,2])
```
```{r AMDmedian, echo=FALSE}
AMDmedian
```
## 4 Time series plot of the average number of steps taken
```{r}
AMDmean<-aggregate(.~interval,data=AMD,FUN=mean)
library(ggplot2)
  ggplot(data=AMDmean, aes(x=interval, y=steps)) +
  geom_area(colour="red",fill="white")+
  xlab("5-minute interval") +
  ylab("average number of steps taken")
```

## 5 The 5-minute interval that, on average, contains the maximum number of steps

```{r}
 Maxinterval<-AMDmean[which.max(AMDmean[,2]),]
```
```{r Maxinterval, echo=FALSE}
Maxinterval[1,1]
```
## 6 Code to describe and show a strategy for imputing missing data
```{r}
AMDsum<-aggregate(.~date,data=AMD[,1:2],FUN=sum,na.action = na.omit)
hist(AMDsum[,2],breaks = 10,xlab="Number of steps",main = "Total number of steps taken each day",col="lightBlue")
```
## 7 Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends
```{r}
AMD[,2]<-weekdays(as.Date(AMD$date))
AMD$date[AMD$date %in% c("Saturday", "Sunday")]<-"Weekend"
AMD$date[AMD$date %in% c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday")]<-"Weekdays"
averages<-aggregate(steps ~ interval + date,data=AMD,FUN=mean)
library(ggplot2)
 ggplot(data=averages, aes(x=interval, y=steps)) +
geom_area(colour="red",fill="white")+facet_grid(date ~ .)+
xlab("5-minute interval") +
ylab("average number of steps taken")


```

```{r, include=FALSE}
   # add this chunk to end of mycode.rmd
   file.rename(from="coursProjects1.Rmd", 
               to="coursProjects1.md")
```