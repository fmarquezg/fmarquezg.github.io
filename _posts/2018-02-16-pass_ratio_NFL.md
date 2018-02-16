---
layout: post
title: "Testing Pass Ratio as a game predictor in the NFL"
modified:
excerpt: ""
tags: [scraping, R, NFL]
modified: 2018-02-16
comments: true
---




Pass ratio:

AVG [ #passes to player_x / Total passes thrown ]


Looking at residual vs fitted plot, there is no distinctive pattern, so I'll it's safe to call this a linear relationship.


Next, let's make sure the residuals are normally distributed. Since we see close to straight line, it is safe to say they are.


In this graph we test our assumption of equal variance (homoscedasticity). At simple glance, points look equaly spread out. 



8,23,30 are causing issues on my model. 

| point        | Team     | Pass Ratio | Wins | PR Rank
| ------------- |:-------------:| -----:| -----:| -----:|
| 8     | CLE | 0.058 | 0 | 31st |
| 23     | NYG |   0.047 | 3 | 13th |
| 30     | TB | 0.041 | 5 | 2nd |

These teams had a very bad 2017, no doubt. 

As any respectable dad would say, 'Defense wins championships' Bingo! Tampa Bay had the worst defense, followed  by NYG. Cleaveland's D was ranked 19, so considering that Cleaveland is ranked 13th on pass ratio, then it makes sense. There is hope! Maybe I shouldn't be saying Pass Ratio wins games, but maybe puts points on the board?



