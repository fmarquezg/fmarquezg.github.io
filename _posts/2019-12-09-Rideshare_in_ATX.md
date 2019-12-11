---
layout: post
title: "Effect of Ridesharing on Austin's DWI records"
modified:
excerpt: ""
tags: [R, Data Science, Uber, Lyft, ATX]
modified: 2019-12-09
comments: false
---

Ridesharing (Uber and Lyft) entered the Austin, Texas market and paused operations for 12 months starting May 2016. Those interested in having ridesharing back in the ATX claimed that the number of DWIs decreased when ridesharing services were available. In this post I tried to validate this claim as on the one hand, it makes sense to see fewer DWIs when ridesharing is available, but on the other hand, I hope people are smart enought to not drink-and-drive regardless of which services are available to them.


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

My solution was to use the Public Intoxication (PI) count in Austin as the control, and run a difference-in-differences regression. Austin's PI count serves as a control because it accounts for Austin's growing population, and is related to the consumption of alcohol.

<p><br></p>

The graph below shows the similar trends in DWI and PI monthly arrests. 

<figure>
	<a href="/images/ridesharing_post/crimes_plot.png"><img src="/images/ridesharing_post/crimes_plot.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

### Difference-in-differences

\\(Arrests = \beta + \beta_{1} * DWI + \beta_{2} * PostRS + \beta{3} * DWI * PostRS \\)


```R
data<-df%>%
  rename(crime_type=`Highest Offense Description`,
         occ_datetime=`Occurred Date Time`)%>%
  mutate(datetime=mdy_hms(occ_datetime))%>%
  mutate(date=date(datetime))%>%
  mutate(month=floor_date(date,unit='month'))%>%
  mutate(rs = case_when((date>='2016-05-01') & (date<'2017-06-01') ~ 0,
                       TRUE ~ 1))%>%
  select(month,date, rs,crime_type)%>%
  filter(month>='2015-05-01', month<'2017-03-01')%>%
  select(-date)%>%
  mutate(crime_type = factor(crime_type, levels = c("PUBLIC INTOXICATION","DWI")),
         rs = as.factor(rs))%>%
  group_by(month,rs,crime_type)%>%summarize(events = n())
  
dreg <- lm(events ~ rs + crime_type +rs*crime_type, data =data)
summary(dreg)
```

```R
Call:
lm(formula = events ~ rs + crime_type + rs * crime_type, data = data)

Residuals:
   Min     1Q Median     3Q    Max 
-49.75 -18.75  -3.90  15.25  59.25 

Coefficients:
                  Estimate Std. Error t value Pr(>|t|)    
(Intercept)        188.300      7.683  24.508  < 2e-16 ***
rs1                 18.200     10.403   1.750   0.0879 .  
crime_typeDWI      102.200     10.866   9.406  1.1e-11 ***
rs1:crime_typeDWI  -16.950     14.712  -1.152   0.2561    
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 24.3 on 40 degrees of freedom
Multiple R-squared:  0.804,	Adjusted R-squared:  0.7893 
F-statistic:  54.7 on 3 and 40 DF,  p-value: 3.243e-14
```


## Resources

* https://www.kut.org/post/have-ride-hailing-services-uber-and-lyft-reduced-drunk-driving-arrests-austin

* https://www.kut.org/post/uber-and-lyft-say-theyll-hit-road-austin-again-monday
