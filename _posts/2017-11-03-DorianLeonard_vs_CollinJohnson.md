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

Conversions in this study are defined by the following:

* If 1st or 2nd down: Receiver catches the ball and gains at least 3 yards (or Touchdown)
* If 3rd or 4th down: Receiver catches the ball gains the yards needed for the 1st down (or Touchdown)
  
## XWR Receptions

| Player        | Times Targeted       | Conversions | Conversion Rate | Yrds Gained |
| ------------- |:-------------:| -----:| -----:| -----:|
| Dorian Leonard      | 25 | 13 | 52.0% | 126 |
| Collin Johnson      | 66 |   34 | 51.5% | 575 |

To keep the plots kosher (and easy for me to recycle code) I'll user the following nomenclature:

  * A: Dorian Leonard (DL) Sr.
  * B: Collin Johnson (CJ) So.
  
In the first two plots below, I show the probability distriution for the conversion rate of receiver A and B. The third plot, is the difference between A and B, in this case Dorian Leonard vs Collin Johnson. 

The x-axis is the conversion rate (cvr) which spans anywhere form 0 to 1, and the y-axiss represents the probability of each x being the 'true' conversion rate. This should read like any probability distribution graph, the highest point represents the most probable point.

<figure>
     <img src="/images/XWR_17/cvr_posteriors.png">
    <figcaption></figcaption>
</figure>

As we can see, both receivers' conversion rate tends to be close to 50%, but Dorian Leaonard's conversion-rate distribution is fatter than Collin Johson. This happens because the sample size of Collin Johson is larger than Leonards sample size (66 times targeted vs 25 times targeted) and thus the error on our assesment of Leonard is larger. 


Taking both distributions into account and using delta as our assesment metric, we conclude the probability Dorian Leaonard's conversion rate being better than Collin Johnson is 51%.


## Let's look at clutch downs only
Let's take this anlysis one steep deeper and look at 3rd down and 4th down situations. This will reduce our sample sizes.

As we can see in the table below, Collin Johson was targeted 18 times and converted in 6 of those attemps. Dorian Leonard on the other hand has 0 conversions and was targeted 3 times. 3 times! Bayesian Inference tends to do well in scenarios where data is limited so let's see how we do in this extreme situation.

## 3rd & 4th Down XWR Receptions

| Player        | Times Targeted       | Conversions | Conversion Rate | Yrds Gained |
| ------------- |:-------------:| -----:| -----:| -----:|
| Dorian Leonard      | 0 | 3 | 0% | 0 |
| Collin Johnson      | 18 |   6 | 33% | 108 |

We can see the distribution for Dorian Leonard appears to be decreasing linearly. Keep in mind the posterior probabilities for each receiver is a Uniform distribution \\( U(0,1) \\)

<figure>
     <img src="/images/XWR_17/3rd_cvr_posteriors.png">
    <figcaption></figcaption>
</figure>


## Appendix


### Methodology

For this study, I started with Uniform distributions as my posteriors.  I decided to go with NUTS as to produce the posterior sample. There wasn't any major difference with Metropolis. 


``` python
with pm.Model() as model:
    #prior
    p_A = pm.Uniform("p_A", 0, 1)
    p_B = pm.Uniform("p_B", 0, 1)
    
    # Deterministic delta function.
    delta = pm.Deterministic("delta", p_A - p_B)

    
    # Set of observations
    obs_A = pm.Bernoulli("obs_A", p_A, observed=observations_A)
    obs_B = pm.Bernoulli("obs_B", p_B, observed=observations_B)

    step = pm.NUTS()
    trace = pm.sample(20000, step=step)
    burned_trace=trace[1000:]
```

### Validation
For those who need the converge test in the above analysis...

A)

<figure>
     <img src="/images/XWR_17/CVR_converge_proof.png">
    <figcaption></figcaption>
</figure>

