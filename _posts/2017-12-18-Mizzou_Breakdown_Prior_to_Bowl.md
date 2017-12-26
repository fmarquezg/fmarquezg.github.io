---
layout: post
title: "Breakdown of Mizzou's last 6 games prior to Texas Bowl"
modified:
excerpt: ""
tags: [Python, data mining, analytics]
modified: 2017-12-16
comments: true
---

Mizzou is is well balanced team. They tend to prefer running the ball.

Ish Witter, their biggest threat as a RB and Emanuel Hall an explosive WR who can make the big play and gain the big yards in a single play.

Key is to cover Tight End Albert Okwuegbunam (Fr) who has 11 touchdowns and averages 14.9 yrds.Okwuegbunam's best performances were against Vanderbilt, where he received for 116 yards and against Arkansas, where he received for 63 yards.


| Down        | Play Type     | Percentage Called |
| ------------- |:-------------:| -----:|
| 1st     | Pass | 42.20% | 
| 2nd     | Pass |   43.33% |
| 3rd     | Pass | 53.33% | 
| 4th     | Pass |   2.43% |

In the following performance breakdown, I compare last 6 game average (AVG), Arkansas game, and Georgia game. Georgia is the only team that managed to keep Mizzou under 30 points.

## Top Rushers

<figure>
     <img src="/images/Mizzou_Breakdown/RB_down.png">
    <figcaption></figcaption>
</figure>

When Rushing, two players stand out. Ish Witter with 119 carries and 727 yards gained and Larry Roundtree III with 83 carries and 401 yards gained.

Ish Witter Yards gained.

| Down        |AVG    | vs ARK | vs UGA |
| ------------- |:-------------:| -----:| -----:|
| 1st     | 6.10 | 4.82 |  1.66 |
| 2nd     | 5.96 |   4.21 | 1.0 |
| 3rd     | 5.44 | 5.8 |  2.0 |
| 4th     | 1.0 |   NA | NA |


Larry Rountree III yards gained.

| Down        |AVG    | vs ARK | vs UGA |
| ------------- |:-------------:| -----:| -----:|
| 1st     | 5.98 | 3.63 |  5.0 |
| 2nd     | 5.09 |   5.33 | 4.0 |
| 3rd     | 14.0 | 3.0 |  5.0 |
| 4th     | 1.0 |   1.0 | NA |





## Top Receivers

<figure>
     <img src="/images/Mizzou_Breakdown/receivers_comp.png">
    <figcaption></figcaption>
</figure>

Mizzou's top receivers are Emanuel Hall, J'Mon Moore, and Albert Okwuegbunam. I attemped to get data on ball drops and reception radius, but the data source didn't really provide details to infer. With the lack of information, I simply compute the ratio of completed passes. Hall and Okwuegbunam completed receptions at about 36% rate. The real MVP was J'Mon Moore, who completed receptions 54% of the time he was targeted. 





Emanuel Hall yards gained.

| Down        |AVG    | vs ARK | vs UGA |
| ------------- |:-------------:| -----:| -----:|
| 1st     | 18.0 | 28.0 |  63.0 |
| 2nd     | 5.3 |   0.0 | 3.0 |
| 3rd     | 16.8 | 55.0 |  36.0 |
| 4th     | NA |   NA | NA |

J'Mon Moore yards gained.

| Down        |AVG    | vs ARK | vs UGA |
| ------------- |:-------------:| -----:| -----:|
| 1st     | 8.6 | 9.4 |  8.0 |
| 2nd     | 10.6 |   10.3 | NA |
| 3rd     | 9.8 | 0.0 |  0.0 |
| 4th     | NA |   1.0 | NA |

Albert Okwuegbunam yards gained.

| Down        |AVG    | vs ARK | vs UGA |
| ------------- |:-------------:| -----:| -----:|
| 1st     | 10.63 | 12.5 |  0.0 |
| 2nd     | 8.88 |   19.0 | 4.0 |
| 3rd     | 11.33 | 9.5 |  6.0 |
| 4th     | 0 |   NA | NA |



