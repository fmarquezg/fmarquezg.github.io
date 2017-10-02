---
layout: post
title: "Chicharito with moves like Jagger?"
modified:
excerpt: "Author analyses the heatmaps of CH14 and compare them against other players"
tags: [Python, data mining, analytics]
modified: 2017-09-30
comments: true
---

Comparisons are never fair or easy, however we tend to always compare things. In the world of soccer, we compare players and use metrics such as goals, assists, silverwear earned or even contract sizes as ways to quantify which player is 'better'. Most of the times we compare players in the same filed position but as team tactics evolve, it becomes hard to find two players developing the same role.

<br>

In this post I'm proposing a method to identify which players can be compared to each other based on their field positioning. To keep this simple I will compare only players' heatmaps. The control in here will be the greatest striker to bless this Earth.... Javier "Chicharito" Hernandez (aka Chichadios). In other words, I will discover which players move around the same areas as Hernandez and thus, which player stats can be 'fairly' compared.

<br>

I will compare his club heatmaps during the 2016-2017 season with the heatmaps produced by other top players to see which players can be 'fairly' compared to Hernandez.

<br>

###The Analysis 

I first collected heatmaps from players from www.whoscored.com. This was a painful task as data is expensive and sites usually create a myriad of roadblocks to prevent people from scraping their site. That being said, I was able to collect about 40 heatmaps per player. This will affect the accuracy of the analysis.

<br>

Players included in this analysis are:
 
* Madzukic
* Arturo Vidal
* Lewandoski
* Edinson Cavani
* Zlatan Ibrahimovich
* Romelu Lukaku
* Gonzalo Higuain
* Luis Suarez
* Marco Verratti
* Kylian Mbappe
* Luka Modric
* Toni Kroos
* MagIsco Alarcon
* Cristiano Ronaldo
* Radamel Falcao
* Dybala

Next I constructed a Convouted Neural Network (CNN) for categorical data. Once the CNN is trained and cross-validated, I then fed a series of heatmaps of Herandez. The output was a list with the probabilities of the heatmap belonging to a player studied in the CNN.


# Results

As we can see in this graph, Hernandez' heatmaps look very similar to Suarez' heatmaps. Notice the value is close to .40, meaning we're 40% sure the heatmaps will belong to those of Suarez. Also note the values for Verratti and Dybala. I am aware they are not strikers, and they move more around midfield. As expected the model gave low values for those two lads.



<figure>
     <img src="/images/ch14_heatmaps_study/overall_sumnary.png">
    <figcaption></figcaption>
</figure>


The charts below plot the average heatmap of each of the top 3 ranked players by the model and puts them next to the average of Hernandez. A simple glance will agree with the results of the model. Suarez and Hernadez produce similar movements!



<figure>
     <img src="/images/ch14_heatmaps_study/ch_suarez.png">
    <figcaption></figcaption>
</figure>

<figure>
     <img src="/images/ch14_heatmaps_study/ch_lukaku.png">
    <figcaption></figcaption>
</figure>

<figure>
     <img src="/images/ch14_heatmaps_study/ch_falcao.png">
    <figcaption></figcaption>
</figure>


Now, I must come clean... I used one season worth of data per player. Injuries, or manager decisions could have prevented the player to start a game which reduced my sample set. Overall, I was able to train my CNN with around 25 heatmaps per player. This was enough to produce a reasonable output, but in order to increase the accuracy and confidence I would have to retrain the network with around 100 heatmaps per player.

Source of heatmaps: www.whoscored.com 


## Footnote
### CNN Settings

{% highlight python %}
%matplotlib inline
train_datagen = ImageDataGenerator(
        rescale=1./255,
        shear_range=0.2,
        zoom_range=0.2,
        horizontal_flip=True,
        )

test_datagen = ImageDataGenerator(rescale=1./255)

training_set = train_datagen.flow_from_directory(
        'Soccer_Heatmaps/dataset/training_set',
        target_size=(64, 64),
        #target_size=(300, 200),
        batch_size=32,
        class_mode='categorical',
        shuffle=False)

test_set = test_datagen.flow_from_directory(
        'Soccer_Heatmaps/dataset/test_set',
        target_size=(64, 64),
        #target_size=(300, 200),
        batch_size=32,
        class_mode='categorical')

classifier.fit_generator(
        training_set,
        steps_per_epoch=10,
        epochs=100,
        verbose=2,
        validation_data=test_set,
        validation_steps=1)
{% endhighlight %}
