---
layout: post
title: "Predicting commitments in the NCAAF 2018 Recruitment class"
modified:
excerpt: ""
tags: [NCAAF, R, RevolUTion]
modified: 2018-06-5
comments: true
---

Recruiting is the essence of College Football. 



In this analysis I'm pulled the player timeline records from <a href="https://247sports.com/Season/2018-Football/" target="_blank">247Sports</a>. 

Georgio, Ohio State, Texas, South California, and Clemson finished in the top 5. Basing myself exclusively in the data available in this site, I 


Recruiting events tracked are:

1. Offer
2. School Camp
3. Unofficial Visit
4. Official Visit
5. Coach Visit
6. Decommit


On average, recruits received 12.7 offers. As the plot below shows, the offer count seems to be all over the place.

2.32 School Vistis, 6.19 Unoficial Visits, 1.97 Official Visits, 3.04 Coach Visits, 


Using this information, how well can schools predict if a recruit is commiting?



To do so 


After extensive data manipulations, I create at table where 

<p><br></p>

\\( P( y =1 | X) = h_\theta(X)=  \sigma(\theta^T X \\)


Where the logit is:

\\(logit[\pi(X)] = \Beta_SC X + \Beta_oVisit X+ \Beta_uVisit X+ \Beta_cVisit X+ \Beta_inState X \\)

In this case 

<p><br></p>
\\( \beta X = \beta _{0} +\beta _{1}X _{per \, capita \, income} + \beta _{2}X _{median \, age}+\beta _{3}X _{total \, population}+\beta _{4}X _{total \, population \times median \, age} +\beta _{5}X _{per \, capita \, income \times median \, age}  \\)


\\( \beta X = \beta _{0} +\beta _{1}X _{per \, capita \, income} + \beta _{2}X _{median \, age}+\beta _{3}X _{total \, population}+\beta _{4}X _{total \, population \times median \, age} +\beta _{5}X _{per \, capita \, income \times median \, age}  \\)

<p><br></p>


Finally, I'm going to add School weights to see if schools take advantage of features. Particularly, I'm intereted in seeing if a School does a better job at keeping in-state talent, or if schools wow their prospects during their visists. 

To do this, I'm changing the original equation so that it looks like this...
