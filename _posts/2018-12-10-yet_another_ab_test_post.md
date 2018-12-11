---
layout: post
title: "Yet another A/B testing post"
modified:
excerpt: ""
tags: [R, Data Science, AB Testing]
modified: 2018-12-10
comments: false
---


I'm not trying to fed a fed horse (I had to) with this post, but there's nothing more dangerous than drawing conclusions from a poorly excecuted AB test. So I'm gonna post a quick tutorial on how to design and experiment and explain the reason behind each step.

<br>

## p-value
The p-value is a powerful statistic that can be interpreted as the probability of an event equal to or more extreme than the observed one occurring when the Null Hypothesis (\\(H_{0})\\) is TRUE. We tend to reject the Null Hypothesis (\\(H_{0})\\) when the computed p-value is lower than 0.05 because the event is very unlikely to occur.
<br>

It is very common to see people run A/B tests and draw conclusions based on wether the p-value printed in an R terminal is less than 0.05. However, this might be misleading as the computed p-value changes drastically as new data is entered.

## Example
Consider the following example

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
