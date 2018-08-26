---
layout: post
title: "Dickson and Tucker in a team might be the ultimate NFL hack!"
modified:
excerpt: ""
tags: [NFL, R, Punter]
modified: 2018-08-26
comments: false
---


Punter (aka Michael Dickson) is showing spectacular punting skills by pinning teams in their own 10th yard on a regular basis. Because of his success I began wondering about what would happen if Punter and Justin Tucker played on the same team. Can the Ravens or Seahawks have Punter make the opposing team start a drive deep in their own field, have their defense force the opposing team to punt and then have their offense move the ball a few yards before having Tucker kicking in a 50 yard field goal? 

<br>

Using <a href="https://github.com/ryurko/nflWAR" target="_blank">nflWAR's</a> 2017 data I got a solid idea of how effective and realistic this strategy could be.

## Drive Outcomes

Most drives end up in punts. During the 2017 season, 53% of drives ended in punts.

<p> 
<figure>
     <img src="/images/punter/outcome_plot.png" width="500" height="100">
    <figcaption></figcaption>
</figure>

The interesting part is, drives starting within the own 15th yard line ended in punts **63%** of the time. This is 10% more than the average punt frequency! 

<br>

## Getting out of the red Zone

Being deep in your goal zone is risky, if you're lucky to not have to worry about a safety, you still have to be cautious of a pick 6, so drives tend to be more conservative. A quick analysis showed drives starting from within the 15th yard line, traveled an average of 26 yards before punting on 4th down. 

<br>

Those punts travelled on average 41 effective yards (effective yards = punt distance - return yards +/-penalty yards). 

<p> 
<figure>
     <img src="/images/punter/effective_punt.png" width="500" height="100">
    <figcaption></figcaption>
</figure>
    
<br>

Adding the 26 travel yards and the 42 effective punt yards to the initial position (15) we end up in our own 17th yard. 

## Yards needed for J Tucker to score

Justin Tucker, a fantasy favorite, missed only 3 field goald in 2017, 2 of those were 50+ field goals. As we can see from the plot below, if the ball gets snapped anywhere within the opposint team's 40th yard line, Tucker will score.


<figure>
     <img src="/images/punter/justin_tucker_goals.png" width="500" height="100">
    <figcaption></figcaption>
</figure>

<br>

So quick math... if the Ravens/Seahawks start from their 17th, move the ball at least 43 yards they will walk away with at least 3 points.
<br>

Okay, maybe teaming up Punter and Tucker won't be the ultimate hack that will break the NFL, BUT it could cut some slack on both sides of the ball.  After all, a good punt does increases the chances of a forced punt, and moving the ball 40 yards shouldn't be too hard.

<br>

<iframe src="https://giphy.com/embed/ToMjGpKniGqRNLGBrhu" width="480" height="256" frameBorder="0" class="giphy-embed" allowFullScreen></iframe><p><a href="https://giphy.com/gifs/jim-carrey-dumb-and-dumber-so-youre-telling-me-theres-a-chance-ToMjGpKniGqRNLGBrhu"></a></p>
 
