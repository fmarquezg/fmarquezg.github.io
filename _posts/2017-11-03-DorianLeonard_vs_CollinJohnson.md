---
layout: post
title: "Comparing the two WR competing for the XWR Slot"
modified:
excerpt: "Author analyses the Amanti Formean's game performance with other WR in the team"
tags: [Python, data mining, analytics]
modified: 2017-10-22
comments: true
---

I compare Dorial Leonard (Sr.) and Collin Johnson's (So.) Game Day Perfomrance. To see which receiver should be the starting. The data accounts for the 2017 Season up to the TCU game.


  
## XWR Receptions
| Player        | Times Targeted       | Conversions | Conversion Rate | Yrds Gained |
| ------------- |:-------------:| -----:| -----:| -----:|
| Dorian Leonard      | 25 | 13 | 52.0% | 126 |
| Collin Johnson      | 66 |   34 | 51.5% | 575 |

To keep the plots kosher (and easy for me to recycle code) I'll user the following nomenclature:

  * Receiver A: Dorian Leonard (DL) Sr.
  * Receiver B: Collin Johnson (CJ) So.
  
In the plots below, I show the probability distriution for the conversion rate of each receiver, and the third plot, is the difference between A and B, in this case Dorian Leonard vs Collin Johnson. The x-axis is the cvr which spans anywhere form 0 to 1, and the y-axiss represents the probability of each x being the 'true' conversion rate.

<figure>
     <img src="/images/XWR_17/cvr_posteriors.png">
    <figcaption></figcaption>
</figure>

As we can see, both receivers' conversion rate tends to be close to 50%, but Dorian Leaonard's conversion-rate distribution is fatter than Collin Johson. This happens because the sample size of Collin Johson is larger than Leonards sample size. (66 times targeted vs 25 times targeted). The conclusion here is; we are more confident about Collin Johnsons cvr, than that of Dorian Leonard.


The probability Dorian Leaonard's conversion rate being better than Collin Johnson is 51%.


## One step deep
Let's take this anlysis one steep deeper and look at 3rd down and 4th down situations.



## 3rd & 4th Down XWR Receptions
| Player        | Times Targeted       | Conversions           | Conversion Rate           |
| ------------- |:-------------:| -----:| -----:|
| Dorian Leonard      | 25 | 13 | 52.0% |
| Collin Johnson      | 66 |   34 | 51.5% |



Check yardages

Check last 2 games (ackwoledge shitty start, CJ is a sophomore 

**The probability of Dorian Leaonard being better than Collin Johnson is 51%**, so this is pretty much a 50-50. 



### Validation
For those who need the converge test in the above analysis...

A)

<figure>
     <img src="/images/XWR_17/CVR_converge_proof.png">
    <figcaption></figcaption>
</figure>

