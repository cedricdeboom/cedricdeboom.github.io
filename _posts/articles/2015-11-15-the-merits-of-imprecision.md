---
layout: post
title: "The Merits of Imprecision"
excerpt: "Short summary about my master's thesis on imprecise probabilities and Hidden Markov Models."
categories: articles
tags: [popular science, university, thesis, paper, markov, probabilities]
comments: true
share: true
image:
  feature: imprecision-header.png
---

_"Around the morning cumuli will soon arrive, and by noon we expect some drizzle over the west of the country. Luckily this is only for a short period of time: at 5pm things will rapidly clear up."_ That's a daily weather forecast on your favourite radio station. The weather forecaster seems to be very confident in his predictions. After all they are based on computer models which have an enormous amount of computing power. However, these computer models are not always 100 procent exact. What if we would get a completely different weather forecast if we would use a new model only differing slightly from the original model? Wouldn't it be better, in that case, if the forecaster would say to his viewers that there are multiple possibilities for tomorrow's weather?

## Imprecise probabilities

Computer models are used in almost all modern information processing systems. Stock market analysts use such systems to predict whether they should buy or sell, engineers use them to calculate the chance of having a tsunami in Japan, and doctors to diagnose cancer. These models almost always use a probabilistic model: the probability of you suffering from cancer is e.g. 10% if there has been a diagnosed case of cancer within your close family. Probabilities such as these can be learned be processing large amounts of data, or they can be posed by a domain expert. However, there are two big problems with such an approach: first it is not always easy to gather a lot of data in a cheap and fast way, and secondly not even the best expert can tell you if the probability of having a sunny day is either 13% or 14%.

We can easily overcome these troubles by considering so-called imprecise probabilities. For example: _"We are not sure whether you're chance of getting cancer is 10%; you're chance is rather somewhere between 5% en 15%."_ In other words, we admit that we cannot model the application by one 'precise' probabilistic model, but rather by a set consisting of multiple such probabilistic models. The benefit is that applications that make use of these imprecise probabilistic models can make clear whether they are confident about a prediction or not.

## Predicting sequences

Suppose the weather on day one is sunny, cloudy on day two and rainy on day three. Putting it this way we arrive at a sequence of weather states, a so-called state sequence. If we would already know the day on every day of the way, then of course nothing remains to be predicted. Therefore, suppose - by means of a thought experiment - we would lock ourselves up for one week in a room without any windows to look outside, but only equipped with a barometer to measure the air pressure. Based on our observations of the air pressure, we then try to predict the weather of the past week. We do this, you'll never guess it, by means of a probabilistic model: the probability of observing a high air pressure on a sunny day is e.g. 70%, 20% if it is cloudy and 10% on a rainy day. The probability of having a sunny day after a rainy day is e.g. 25%, while the probability of having two subsequent rainy days is 40%. Such a model is also called a [Hidden Markov Model](https://en.wikipedia.org/wiki/Hidden_Markov_model){:target="_blank"} (HMM). That is, the sequence of weather states is _hidden_, but we know the probabilistic relationships between observed and hidden states.

<figure>
	<a href="/images/imprecision/hmm.png"><img src="/images/imprecision/hmm.png" alt="An example visualization of a Hidden Markov Model."></a>
	<figcaption>An example visualization of a Hidden Markov Model, in which we know the sequence of air pressures \(O_{1:3}\) (H or L), and want to find out the hidden sequence of weather states \(X_{1:3}\). The arrows denote a probabilistic dependency.</figcaption>
</figure>

Predicting the sequence of weather states is traditionally done using the famous [Viterbi algorithm](https://en.wikipedia.org/wiki/Viterbi_algorithm){:target="_blank"}, developed at the end of the sixties. The algorithm is used successfully in deep space communication, speech processing, DNA analytics... In our example the algorithm will calculate the most probable hidden sequence of weather states based on our air pressure observations, and this in a fast and efficient way. _Nihil novi sub sole_ thus far...

## Predicting sequences, but more robustly
There are some troubles with the classical approach of modeling and tackling this problem. The classical Viterbi algorithm makes use of a traditional 'precise' probabilistic model. If this model does not completely correspond to reality, how can we be sure then about the predictions made by the algorithm? At this point in the story, imprecise probabilities enter the stage.

In 2013 and 2014, under the supervision of professor Gert de Cooman, PhD, Jasper De Bock, PhD, and Arthur Van Camp, I have devised a more robust, tree-based version of the Viterbi algorithm, called _MaxiHMM_, a sort of 'Viterbi 2.0' algorithm if you like. The algorithm is completely based on the use of imprecise probabilistic models and is comparable to the original Viterbi algorithm performance-wise (that is, they have comparable time complexity). By using imprecise models, the algorithm can easily express its confidence in the predictions it makes. In case MaxiHMM returns multiple solutions that are equally 'probable', we can be less confident in finding the right one than when MaxiHMM only returns one single solution.

Suppose MaxiHMM calculates that the weather on three consecutive days is either sunny-sunny-cloudy or sunny-cloudy-cloudy, and this by using an imprecise model. The algorithm is thus confident about the weather on days one and three, but remains unsure about the weather on the second day, which can be either sunny or cloudy. In this case the end user will get a better image of the possible solutions and to what extent he should be confident about them. This is opposed to using the classic Viterbi algorithm, in which the user only gets presented one optimal solution.

## Applications
The number of possibly applications of the traditional Viterbi algorithm is almost countless, such as speech processing, DNA-sequence analysis, stock rate prediction, etc. Each of these applications can, from now on, also use the imprecise MaxiHMM algorithm. This way the end user gets informed about the confidence he or she can place in the Viterbi solution.

By means of example we created a simple application in the context of music composition. In this the user creates a musical melody, which will be completed with a sequence of chords through the MaxiHMM algorithm. These chords can be used to accompany the melody by e.g. a guitar or a piano. An example is given in the figure below. As a model we used expert knowledge to know which chords often follow each other, and which chords are often placed under certain notes. A big advantage of using MaxiHMM instead of Viterbi, is that MaxiHMM can generate multiple chord sequences as the imprecision increases. You can therefore select the sequence which appeals the most to you, thereby stimulating the creative process. Applications in music are endless; we can for example turn things around and generate a melody given a certain chord sequence, generate fingerings for pianists, etc.

<figure>
	<a href="/images/imprecision/music.png"><img src="/images/imprecision/music.png" alt="An example of a melody with chords generated by the MaxiHMM algorithm."></a>
	<figcaption>An example of a melody with chords generated by the MaxiHMM algorithm.</figcaption>
</figure>

## More information
If you liked the ideas in this article, I can point you to other resources to learn more about this research in particular, and also to general sources on imprecise probabilities:

* The paper [_Robustifying the Viterbi Algorithm_](http://users.ugent.be/~jdbock/documents/CDB-2014-PGM-paper.pdf){:target="_blank"} published in 2014 as a summary of the complete work.
* The complete [master's thesis](http://scriptiebank.be/sites/default/files/webform/scriptie/thesis_15.pdf){:target="_blank"}.
* The books [_Statistical Reasoning with Imprecise Probabilities_](https://books.google.com/books/about/Statistical_reasoning_with_imprecise_pro.html?id=-hbvAAAAMAAJ){:target="_blank"} by Peter Walley (which is currently out of print) and [_An Introduction to Imprecise Probabilities_](http://www.wiley.com/WileyCDA/WileyTitle/productCd-0470973811.html){:target="_blank"} by Thomas Augustin, Frank Coolen, Gert de Cooman and Matthias Troffaes.
