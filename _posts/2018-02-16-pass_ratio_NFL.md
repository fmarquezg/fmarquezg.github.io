---
layout: post
title: "Using a Pass Ratio Metric to predict Air Yard Performance (SPOILER: not a good metric)"
modified:
excerpt: ""
tags: [scraping, R, NFL]
modified: 2018-02-16
comments: true
---


In this post, I'm testing a popular soccer metric, pass ratio, with NFL passer/receiver data and I'll try to determine if this metric can help predict total air yards.  

Pass Ratio was first developed by the mathematicion Thomas Grund. He used it to evaluate soccer matches and discovered that teams that focusEd their passing on a select few players usually scored less goals than teams who distributed their passes evenly. It is important to mark that in soccer, all players receive and pass the ball, while there are some players (center midfield) who tend to distribute the ball the most, all players are involved in passing. Football, as we know, only the QB passes the ball. Ocassionaly, a RB or a kicker may throw the ball on a trick play, but let's ingore those cases as they are not frequent.

The motivation to test this metric, is that if a QB does his receiver progressions well, and has multiple players able to open separate from their coverage, then that QB would be passing to multiple targets and possibly having a better offense. If a QB only trusts a select group of receivers , then the opposing Defense should be able to double cover those safety targets.

### Calculating Pass ratio for NFL :

Sum the number of passes to a player, then compare this to the number of passes made to most active player. 

78-39 = avg this differences throught each game, and then divide by total number of passes


<p><br></p>
\\(PR = \frac{\sum_{i=1}^{total_players} maxPasses - passes_i}{total pases} \\)
<p><br></p>

where:

<p><br></p>

\\( \beta X = \beta _{0} +\beta _{1}X _{per \, capita \, income} + \beta _{2}X _{median \, age}+\beta _{3}X _{total \, population}+\beta _{4}X _{total \, population \times median \, age} +\beta _{5}X _{per \, capita \, income \times median \, age}  \\)

<p><br></p>


AVG [ #passes to player_x / Total passes thrown ]



## Analysis

Analysis is simple, just a linear regression with average pass ratio as the independent variable.

```R
lr_model<-lm(formula=total_wins~avg_pr,data=sum_game_w)
summary(lr_model)

Call:
lm(formula = total_wins ~ avg_pr, data = sum_game_w)

Residuals:
    Min      1Q  Median      3Q     Max 
-5.8728 -1.8706  0.4463  1.8161  4.9955 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept)   20.546      5.164   3.979 0.000405 ***
avg_pr      -252.860    102.760  -2.461 0.019843 *  
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 3.01 on 30 degrees of freedom
Multiple R-squared:  0.1679,	Adjusted R-squared:  0.1402 
F-statistic: 6.055 on 1 and 30 DF,  p-value: 0.01984
```

While there is a negative slope, the R-squared value is too small.


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




