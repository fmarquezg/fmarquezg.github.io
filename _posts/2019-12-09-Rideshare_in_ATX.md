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

In this post I tried to measure the effect (if any) of ridesharing on DWIs using the **CausalImpact** Bayesian time-series model.


### Timeline

* 2013 - Uber and Lyft entered the Austin market, but it wasn't until October of 2014 that the city passed legislation to formally allow their opeartions.

* May 2016 - Politics happened and both Uber and Lyft pulled out

* May 29, 2017 - Uber and Lyft resumed their Austin operations


## Analysis

In this scenario, randomized experiment techniques are not applicable, but a causal inference approach is ideal for this scenario.

CausalImpact assumes the treatment series can be explaned in terms of a control series that is not affected by the treatment or intervention. The challenge now becomes finding an appropriate control series.

<p><br></p>

For my control series I chose to use Austin's Public Intoxication (PI) arrest records. Austin's PI count serves as a control for the following reasons:

* Accounts for Austin's population growth
* Same seasonality (weekly) as DWIs
* Major festivals/hollidays are affected equally
* Is an alcohol related crime


<p><br></p>


### Collect data

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

The plot below suggest both the PI and the DWI series are approriate for the CausalImpact analysis. 

<figure>
	<a href="/images/ridesharing_post/crimes_plot.png"><img src="/images/ridesharing_post/crimes_plot.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

<p><br></p>

### Analysis

After a little tidyversing, I created the following dataframe where the `dwi` and `pi` columns represent daily arrests. 

```R
> data
           dwi pi
2016-03-15   7  8
2016-03-16   4  4
2016-03-17  12  8
2016-03-18  10  6
2016-03-19  13  5

```

For the date range I followed best practice and selected about 45 days before intervention and 30 days post intervention.


```R
library(CausalImpact)
library(tidyverse)

pre.period <- as.Date(c("2016-03-15", "2016-04-30"))
post.period <- as.Date(c("2016-05-01", "2016-06-01"))

impact <- CausalImpact(data, pre.period, post.period,model.args = list(nseasons = 7, season.duration = 1))
plot(impact)
```

The number of DWIs after Uber and Lyft paused activities between 2016-05-01 and 2016-06-01 added up to 286. Had both companies not paused, we would have expected any amount between 210 and 278 (95% confidence interval). This means we observed a higher number of DWIs than expected, and it is highly unlikely the amount observed was the product of simple fluctiations.

<figure>
	<a href="/images/ridesharing_post/ci.png"><img src="/images/ridesharing_post/ci.png"></a>
	<figcaption> Crimes </figcaption>
</figure>


## Resources

* https://www.kut.org/post/have-ride-hailing-services-uber-and-lyft-reduced-drunk-driving-arrests-austin

* https://www.kut.org/post/uber-and-lyft-say-theyll-hit-road-austin-again-monday
