---
layout: post
title: "Using a Pass Ratio Metric to predict Air Yard Performance (SPOILER: not a good metric)"
modified:
excerpt: ""
tags: [pass ratio, R, NFL]
modified: 2018-02-16
comments: true
---


In this post, I'm testing a popular soccer metric, Pass Ratio (PR), with NFL passer/receiver data and I'll try to determine if this metric can help predict total air yards.  

Pass Ratio was first developed by the mathematicion Thomas Grund. He used it to evaluate soccer matches and discovered that teams that focusEd their passing on a select few players usually scored less goals than teams who distributed their passes evenly. It is important to mark that in soccer, all players receive and pass the ball, while there are some players (center midfield) who tend to distribute the ball the most, all players are involved in passing. Football, as we know, only the QB passes the ball. Ocassionaly, a RB or a kicker may throw the ball on a trick play, but let's ingore those cases as they are not frequent.

The motivation to test this metric, is that if a QB does his receiver progressions well, and has multiple players able to open separate from their coverage, then that QB would be passing to multiple targets and possibly having a better offense. If a QB only trusts a select group of receivers , then the opposing Defense should be able to double cover those safety targets.

### Calculating Pass ratio for NFL :

As mentioned before, the Pass Ratio is a way to measure network centrality at a game level on a scale from 0-1. The way I calculated it is in the following

<p><br></p>
\\(PR = \frac{\sum_{i=1}^{TP} maxPasses - passes_i}{Total Pases} \\)
<p><br></p>
Where:
\\( TP: total number of receivers who caught a pass during the game \\)
\\( maxPases: the highest number of passes thrown at one person during the game \\)
\\( TotalPasseS: Total passes thrown by the QB \\)
<p><br></p>

If a QB throws at multiple targets in an even distribution, then team's pass ratio will be rather low. Alternatevely, if a QB, caves in to his diva WR and makes every pass to him, then that team will have a high PR.



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




