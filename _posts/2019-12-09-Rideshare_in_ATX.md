---
layout: post
title: "Effect of Ridesharing on Austin's DWI records"
modified:
excerpt: ""
tags: [R, Data Science, Uber, Lyft, ATX]
modified: 2019-12-09
comments: false
---

Ridesharing (Uber and Lyft) entered the Austin, Texas market and paused operations for 12 months starting May 2016. One of the popular arguments in favor of bringing Uber and Lyft back to Austin was that their service reduced the number of DWI arrests.

<p><br></p>

In this post I tried to measure the effect (if any) of ridesharing on DWIs by running a caual Impact analysis.


### Timeline

* 2013 - Uber and Lyft entered the Austin market, but it wasn't until October of 2014 that the city passed legislation to formally allow their opeartions.

* May 2016 - Politics happened and both Uber and Lyft pulled out

* May 29, 2017 - Uber and Lyft resumed their Austin operations



## Collect data

As usual, the first step is to collect crime data. Luckily the city of Austin has a nice site that allows us to collect this data at scale.

```R
# Code to Pull data
df<-NULL
s<-seq(0:1000000)
for (i in s){
  i<-as.character(i*1000)
  query_url<-paste0('https://data.austintexas.gov/resource/fdj4-gpfu.json?$limit=1000&$offset=',i)
  try(
    dat<-fromJSON(query_url,flatten = TRUE)
  )
  try(
    df<-rbind(dat,df)
  )
```
## Analysis

In order quantify the effect of ridesharing, I need to establish a control. Unfortunately, ridesharing left the city all together, so any type of A/B testing was impossible. I thought about using other cities as controls, but I ran into issues finding cities similar to Austin and when I found similar cities, I discovered that they either didn't have a accesible data or the data simply didn't look accurate.

<p><br></p>

My solution was to use the Public Intoxication (PI) count in Austin as the control. Austin's PI count serves as a control for the following reasons:

* Accounts for Austin's population growth
* Same seasonality (weekly) as DWIs
* Major festivals/hollidays are affected equally
* Is an alcohol related crime


<p><br></p>

The graph below shows the similar trends in DWI and PI monthly arrests. 

<figure>
	<a href="/images/ridesharing_post/crimes_plot.png"><img src="/images/ridesharing_post/crimes_plot.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

### Analysis

```R
library(CausalImpact)
library(tidyverse)
```


```R
datad<-df%>%rename(crime_type=`Highest Offense Description`,
         occ_datetime=`Occurred Date Time`)%>%
  mutate(datetime=mdy_hms(occ_datetime))%>%
  mutate(date=date(datetime))%>%
  mutate(month=floor_date(date,unit='month'))%>%
  mutate(rs = case_when((date>='2016-05-01') & (date<'2017-06-01') ~ 0,
                        TRUE ~ 1))%>%
  select(month,date, rs,crime_type)%>%
  #filter(month>='2014-05-01', month<'2017-05-01')%>%
  #select(-date)%>%
  mutate(crime_type = factor(crime_type, levels = c("PUBLIC INTOXICATION","DWI"))
         #rs = as.factor(rs)
  )%>%
  group_by(date,rs,crime_type)%>%summarize(events = n())

datad2<-day_df%>%left_join(datad,by=c('date','crime_type'))%>%
  mutate(events = ifelse(is.na(events),0,events),
         rs = ifelse(is.na(rs),0,rs))%>%
  mutate(rs = as.factor(rs))%>%
  filter(date>='2016-03-15', date<'2016-06-01')
```

```R
dwi<-datad2%>%filter(crime_type=='DWI')%>%dplyr::select(events)%>%rename(dwi = events)
pi<-datad2%>%filter(crime_type=='PUBLIC INTOXICATION')%>%dplyr::select(events)%>%rename(pi = events)
dates_d<-datad2%>%filter(crime_type=='PUBLIC INTOXICATION')
dates_d<-dates_d$date

data <- zoo(cbind(dwi, pi), dates_d)
min(dates_d)
max(dates_d)
pre.period <- as.Date(c("2016-03-15", "2016-04-30"))
post.period <- as.Date(c("2016-05-01", "2016-06-01"))

impact <- CausalImpact(data, pre.period, post.period,model.args = list(nseasons = 7, season.duration = 1))
plot(impact)
```


<figure>
	<a href="/images/ridesharing_post/ci.png"><img src="/images/ridesharing_post/ci.png"></a>
	<figcaption> Crimes </figcaption>
</figure>


## Resources

* https://www.kut.org/post/have-ride-hailing-services-uber-and-lyft-reduced-drunk-driving-arrests-austin

* https://www.kut.org/post/uber-and-lyft-say-theyll-hit-road-austin-again-monday
