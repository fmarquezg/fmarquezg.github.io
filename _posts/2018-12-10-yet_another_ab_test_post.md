---
layout: post
title: "Yet another A/B testing post"
modified:
excerpt: ""
tags: [R, Data Science, AB Testing]
modified: 2018-12-10
comments: false
---


I'm not trying to feed a fed horse (I had to) with this post, but there's nothing more dangerous than drawing conclusions from a poorly excecuted AB test. This post is will detail the factors to consider when designing an AB Test and the reasoning behind each factor.

<br>

## Null Hypothesis (\\(H_{0})\\)
Coming up with a hypothesis to test is the arguably the most important part of an experimint. There is nothing to test without a hypothesis. The Hypothesis we seek to reject is the Null Hypothesis. A common \\(H_{0})\\ for AB tests is \\(\mu_{A}=\mu_{B}=)\\. Which states the mean of sample A is equal to the mean of sample B.


## p-value
The p-value is a powerful statistic that can be interpreted as the probability of an event equal to or more extreme than the observed one occurring when the Null Hypothesis (\\(H_{0})\\) is TRUE. We tend to reject the Null Hypothesis (\\(H_{0})\\) when the computed p-value is lower than 0.05 because the event is very unlikely to occur.
<br>

It is very common to see people run A/B tests and draw conclusions based on wether the p-value printed in an R terminal is less than 0.05. However, this might be misleading as the computed p-value changes drastically as new data is entered.

## Example
Imagine you want to test whether a coin is fair or not. You flip a coin 10000 times and say you measure the number of times the coin landed in Heads. Using the `binom.test` R function we check the p-value for when our (\\(H_{0}) = 0.5 \\). If the coin is fair, we should observer a large p-value.



```R
set.seed(2)
n=10000
toss <- rbinom(n,1,prob=0.5)
p.values <- numeric(n)
erun<-numeric(n)
for (i in 5:n){
  p.values[i] <- binom.test(table(toss[1:i]),p=0.5)$p.value
  erun[i]<-i
}
df<-data.frame(erun,p.values)
df%>%ggplot(aes(x=erun,y=p.values))+geom_line()+
  geom_hline(yintercept=0.05,color='red',size=1,linetype='dashed',alpha=0.5)+
  labs(x = "Coin Flips", y = "p-value",
       title = "Change in p-value") +
  theme_bw() + theme(axis.text = element_text(size = 10), 
                     title = element_text(size = 10)) 

```

As we can see in the graph, the p-values are larger than the standard threshold of 0.05 throughout the duration of the experiment, but this is not always the case. If we were to repeat the experiment, we might get misleading results.

```R
set.seed(22)
n=10000
toss <- rbinom(n,1,prob=0.5)
p.values <- numeric(n)
erun<-numeric(n)
for (i in 5:n){
  p.values[i] <- binom.test(table(toss[1:i]),p=0.5)$p.value
  erun[i]<-i
}

```

In this example, had we ended our experiment the moment the p-value crossed the 0.05 threhhold, we would wrongfully concluded the coin is biased. 
 
### Solution:
Collect N samples and DON'T PEEK until the experiment concludes.

# Sample Size

\\n = 16 \frac{\sigma^{2}}{\delta^2})\\

where (\\ \delta\\) is the minimum effect we wish to detect. Note that the smaller the effect you wish to detect, the larger your sample should be. Also, having a large sample variance, will result in a large sample size needed.

If you're an R user you can use the following to get a sample size:

```R
library(pwr)
pwr.t.test(n=NULL,d=0.1, sig.level=0.05, power=0.8)

     Two-sample t test power calculation 

              n = 1570.733
              d = 0.1
      sig.level = 0.05
          power = 0.8
    alternative = two.sided

NOTE: n is number in *each* group

```

