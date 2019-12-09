---
layout: post
title: "Did DWIs in Austin increased whithout ridesharing servies"
modified:
excerpt: ""
tags: [R, Data Science, Uber, Lyft, ATX]
modified: 2019-12-09
comments: false
---


* Uber and Lyft entered the Austin market in 2013, but it wasn't until October of 2014 that the city passed legislation to formally allow their opeartions.

* Politics happened and both UBER and LYFT pulled out in May 2016

* Uber and Lyft resumed their Austin operations in May 29, 2017


## Collect data
As usual, the first step is to collect crime data. Luckily the city of Austin has a nice site that allows us to collect this data at scale.

```R
Code to Pull data
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



## Resources
* https://www.kut.org/post/have-ride-hailing-services-uber-and-lyft-reduced-drunk-driving-arrests-austin
* https://www.kut.org/post/uber-and-lyft-say-theyll-hit-road-austin-again-monday
