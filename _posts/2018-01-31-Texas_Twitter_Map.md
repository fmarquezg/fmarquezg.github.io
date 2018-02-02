---
layout: post
title: "Bryan Carrington the recruiting MVP"
modified:
excerpt: ""
tags: [Scala, Twitter, GraphX, Neo4j data mining, analytics]
modified: 2018-01-31
comments: true
---

Twitter is an amazying networking tool (duh!), and the University of Texas Football program seems to be leveraging it to market itself to recruits. In this study I analyzed a Twitter segment (17,916 twitter users), to shed light on landscape of Texas Football to see how important each member of the Texas family is for the network. Spoiler alert - Bryan Carrington is killing it!

<br>


The graph below centers over Bryan Carrington who is the most imprtant actor among the Texas Football staff.

<br>

<figure>
     <img src="/images/Graphs/BC_tweet_map.png">
    <figcaption></figcaption>
</figure>


Mr. Carrington keep doing you and Hook Em'!



## Why is Carrington MVP?

I've said Carrington is the most important member in this Texas Football network, now I'll go over how I reached to that conclusion.

<br>

I ran PageRank to quantify the 'importance' of each user in the network. Without getting too mathy, this algorithm will give a high value to a user who is in contact directly or indirectly by several other users. Not surprisingly, most of the top 20 ranked users are organizations or websites producing content. NFL is not surprisingly the highest ranked twitter handle, because after all, this is a football focused sample.

<br>


AS we start going dowin in the table we see find, ranked 18th out of 18K, we see @BCarringtonUT! He is the highest ranked member of the Texas Football staff. Bryan Carrington scoring high in this analysis means he is strengthening the Texas brand on social media. 




| Rank | Twitter User        | PageRank  |
| ------------- |:-------------:|:-------------:|
 | 1 | NFL | 1.874600971569031 | 
 | 2 | lecrae | 1.8704610172190697 | 
 | 3 | espn | 1.7463025833905075 | 
 | 4 | TexasFootball | 1.7374981226104123 | 
 | 5 | EJHolland247 | 1.68736313734092 | 
 | 6 | SportsCenter | 1.6750906365355547 | 
 | 7 | Fast7v7 | 1.4924840197649325 | 
 | 8 | BleacherReport | 1.454120126564554 | 
 | 9 | 247Sports | 1.453521020573066 | 
 | 10 | RivalsKroogCity | 1.4534474990277153 | 
 | 11 | TexasMBB | 1.4120211660315887 | 
 | 12 | GPowers247 | 1.3983418377842196 | 
 | 13 | CoachMikeGPU | 1.3982839479183191 | 
 | 14 | Perroni247 | 1.3944429939046235 | 
 | 15 | D_JAMISON5 | 1.39141873408719 | 
 | 16 | nflnetwork | 1.3737737452965173 | 
 | 17 | VYPEHouston | 1.3733000616444797 | 
 | 18 | BCarringtonUT | 1.3687110228079715 | 
 | 19 | Godly_Life | 1.3678290130140847 | 
 | 20 | MikeRoach247 | 1.366327305165802 | 





## Appendix

### How was this data collected

I started with 13 twitter handles:

* TheTommyBush11
* _ACook21
* lil_STERNS2
* TexasFootball
* DrewMehringer
* D1__JW
* _BrennanEagles__
* CoachWarehime
* Coach_Naivar
* Vonte_4
* BCarringtonUT
* CoachTomHerman
* Christo24AHFC

I collected the most recent 200 tweets by each user and recorded the twitter handles of the users being tweeted at by the original 13 handles. I then collected the most recent 200 tweets by the users being tweeted at by the original 13 tweeter handles. I repeated this process 20 times have a a decent sized network.


### Top communications

This section shows which user mentioned another the most. Coach Meekins ranked 9 by mentioning TexasFootball 85 times!

* BR_NBA tweeted BleacherReport 106 times
* DB_BucksFB tweeted SleeperAthletes 98 times
* rivalsmike tweeted Rivals 96 times
* CampbellGators tweeted CyFairISD 93 times
* BobLabriola tweeted steelers 91 time
* ghaugii7 tweeted wyo_football 88 times
* dfwvarsity tweeted Gosset41 87 times
* VYPEInnerLoop tweeted emcstats 86 times
* corbymeekins tweeted TexasFootball 85 times
* CampbellGators tweeted Dr_PWilliamsJ 84 times



