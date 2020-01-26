---
layout: post
title: "Stop using for loops in R"
modified:
excerpt: "I compared performance of using for loops."
tags: [R, Rstats, analytics]
modified: 2020-01-25
comments: true
---

Many have mentioned the use of "for loops" in R is suboptimal. We are all taught about for loops when we begin coding because they work and are easy to follow. However, computers hate it. 


I decided to check this out by benchmarking execution time and comparing a **for loop** with a **do.call** alternative in R.



```R
library(microbenchmark)

loop_f <- function(x){
  opt<-c()
  for (i in seq(1:x)){
    opt[i] <-i+1
  }
  return(opt)
}


docall_f <-function(x){
  x<-list(1:x)
  f <-function(x) return(x+1)
  out<-do.call(f,x)
  return(out)
}
```

Moment of truth, run the benchmark test.

```R
microbenchmark(
  loop_f(100),
  docall_f(100)
)
``` 

Results are quite shocking. Even for a very simple function, the performance is 10x (median) better when we use **do.call** instead of looping through the data.


### Conclusion
Stop using for loops when possible.
