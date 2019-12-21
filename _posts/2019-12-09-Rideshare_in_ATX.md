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

In this post I tried to measure the effect (if any) of ridesharing on DWIs using the <a href="https://github.com/google/CausalImpact" target="_blank"> CausalImpact </a> Bayesian time-series model.


### Event Timeline

* 2013 - Uber and Lyft entered the Austin market, but it wasn't until October of 2014 that the city passed legislation to formally allow their opeartions.

* May 2016 - Politics happened and both Uber and Lyft pulled out

* May 29, 2017 - Uber and Lyft resumed their Austin operations


## Analysis

When Uber and Lyft halted operations in Austin, they did so in an instantaneous fashion. They did not stop operations in a specific Zip Code or periodically reduced the number of drivers, they basically just shut down on from one day to another. In this scenario, measuring the effect of ridesharing on DWIs is not possible via randomized experiment techniques was not possible, but a causal inference approach is ideal for this scenario.

<p><br></p>

CausalImpact assumes the treatment series can be explaned in terms of a control series that is not affected by the treatment or intervention. The challenge now becomes finding an appropriate control series.

<p><br></p>

### Controls

For my control series I chose arrest counts for other crimes commited in Austin. The arrest count comes from the same source as the DWI counts. The figure below plots the treand line for arrests broken down by crime type.


<p><br></p>




<figure>
	<a href="/images/ridesharing_post/all_atx_crimes.png"><img src="/images/ridesharing_post/all_atx_crimes.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

<p><br></p>


From the figure above, I selected 5 Crimes that were trending in a similar fashion as DWIs. When selecting those 5 controls I was looking for controls that met the following criteria:

* Control must account for a growing population
* Control must have a higher occurence during weekends
* Events must be independent of any ridesharing available
* I wanted to eyeball a similar (or opposite) trend as the DWI trend

The figure below shows the selected controls. 

<figure>
	<a href="/images/ridesharing_post/select_crimes.png"><img src="/images/ridesharing_post/select_crimes.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

### Testing Hypothesis

After a little tidyversing, I created a zoo object that included the DWI count followed by the other controls.

```R
data <- zoo(cbind(DWI,PI, ABC, AT, CM,FD), dates_d)
data<-na.fill(data,0)
```

```R
> data
           DWI PI ABC AT CM FD
2016-02-20  23  8   3  5 22 27
2016-02-21  10 12   1  4 21 30
2016-02-22  10  6   5  2 15 20
2016-02-23   7  5   5  3 19 19
2016-02-24   6  5   3  5 15 22
2016-02-25   8  4   5  2 17 15

```

The folks at google made the implementation of CausalImpact incredibly easy. 

```R
pre.period <- as.Date(c("2016-02-20", "2016-04-30"))
post.period <- as.Date(c("2016-05-01", "2016-06-01"))

impact <- CausalImpact(data, pre.period, post.period,alpha=0.1,model.args = list(niter = 10000,nseasons = 7, season.duration = 1, prior.level.sd=0.1))

impact <- CausalImpact(data, pre.period, post.period,alpha=0.1,model.args = list(niter = 10000,nseasons = 7, season.duration = 1, prior.level.sd=0.1))

plot(impact)
```


<figure>
	<a href="/images/ridesharing_post/daily_causal_impact.png"><img src="/images/ridesharing_post/daily_causal_impact.png"></a>
	<figcaption> Crimes </figcaption>
</figure>




The result is a bit surprising. There is no evidence to support any effect of an increase or decrease in the count of DWIs after Uber and Lyft paused operations in Austin. The cumulative plot (bottom 3rd) plots the cumulative difference between the actual DWI count and the estimated count had nothing changed. The probability of observing the *post* count of DWIs had Uber and Lyft remained in Austin is of 41%. As we can see, we are safe saying there was no clear effect.

#### Weekly Aggregation

Given the noise originating from having daily data points, I tried running this same analysis but on a weekly basis.

<figure>
	<a href="/images/ridesharing_post/weekly_select_crimes.png"><img src="/images/ridesharing_post/weekly_causal_impact.png"></a>
	<figcaption> Crimes </figcaption>
</figure>

The conclusion is consistent, no evidence to suggest a change in DWI counts. The probability of observing an effect similar to the one observed by chance based on the *pre* data data is 34%.

<figure>
	<a href="/images/ridesharing_post/weekly_causal_impact.png"><img src="/images/ridesharing_post/daily_causal_impact.png"></a>
	<figcaption> Crimes </figcaption>
</figure>


## Summary

<p><br></p>
This analysis failed to measure a change in DWIs after Uber and Lyft paused operations in Austin. Looking at data on a weekly and daily basis yielded the same results. It is important to acknowledge that while Uber and Lyft did pause operations, there were some smaller ridesharing alternatives entering the market (i.e.  Fasten, Fare, RideAustin, Random Facebook groups, craigslist). Saying ridesharing didn't exist in Austin after Uber and Lyft left would be incorrect, and maybe the reason why we don't detect a significant increase in DWIs is because there were ridesharing options available to those who would otherwise be dumb enough to drink and drive.

<p><br></p>

Notes - The full code can be found on my github (<a href="https://github.com/fmarquezg/ride_sharing_austin/blob/master/causalImpact_ridesharing.R" target="_blank"> daily </a>, <a href="https://github.com/fmarquezg/ride_sharing_austin/blob/master/causal_impact_ridesharing_weekly.R" target="_blank"> weekly </a>).


## Resources

* https://www.kut.org/post/have-ride-hailing-services-uber-and-lyft-reduced-drunk-driving-arrests-austin

* https://www.kut.org/post/uber-and-lyft-say-theyll-hit-road-austin-again-monday
