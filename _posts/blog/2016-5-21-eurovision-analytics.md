---
layout: post
title: "Messing with Eurovision data"
excerpt: "Why Russia is the only true winner of the Eurovision song contest"
categories: blog
tags: [data, eurovision]
comments: true
share: true
image:
  feature: eurovision-header.jpg
  credit: YouTube
---

Last week the traditional Eurovision song contest was held in Stockholm, Sweden. The favourites to win this year's edition, and also to host next year's, were Russia, Ukraine, Sweden and Australia, at least according to the bookmakers[^1]. After a fantastic debut last year, Australia again took part in the competition.

The release of the voting results was exciting this year, especially since all the jury points were gathered first, before the televotes. It is known that 'professional' juries tend to vote - sometimes completely - differently than the masses, so things could quickly turn around. For those of you who do not follow Eurovision or are not familiar with the scoring, a quick recap. Every country can vote for other countries (except for their own), and the ten best get points: 1, 2, 3, 4, 5, 6, 7, 8, 10 and 12. Then all points get summed to arrive at the total scores. Australia was clearly in the lead after the jury votes were counted, as is shown by this graph:

<figure>
	<a href="/images/eurovision/jury-votes.png"><img src="/images/eurovision/jury-votes.png" alt="The jury votes"></a>
	<figcaption>The combined jury votes. Australia is in the lead with 320 points.</figcaption>
</figure>

My own country, Belgium, is in a comfortable fifth place, an ex aequo with Russia. After the jury points were made public, the points from the televoters were added. The televote points are shown in the following graph:

<figure>
	<a href="/images/eurovision/tele-votes.png"><img src="/images/eurovision/tele-votes.png" alt="The televotes"></a>
	<figcaption>The combined televotes. Now Russia is in the lead.</figcaption>
</figure>

Now Russia and Ukraine are in the lead, with 361 and 323 points respectively. Please also take a look at Poland in both graphs. The country got almost no points from the professional juries, but is in a remarkable third place among the televoters, with 222 points!  Belgium only got 51 points from the televoters. It shows that indeed 'professionals' and non-professionals have other opinions. It is also interesting to see that the televote graph resembles an exponential distribution much more than the jury graph. When the jury and televote points are added, we get the following graph:

<figure>
	<a href="/images/eurovision/total-votes.png"><img src="/images/eurovision/total-votes.png" alt="A total of the jury and televote points"></a>
	<figcaption>The total points. Ukraine is the winner.</figcaption>
</figure>

Although close, Ukraine became the winner of the 2016 Eurovision song contest with 534 points, although it did not came first in both the jury and televote rankings. Australia got 511 points, and Russia 491.

The thing that has always bothered me with Eurovision, is that every country gets an equal vote, no matter the size or population of the country. For example, France had a population of 66 million people in 2014, but it is equally represented in the votes than, let's say, Montenegro with a population of only 600 thousand in 2014. What would happen if every European gets an equal vote, instead of every country? To test this, we can weigh the points of every country by the country's population. This is of course only an approximation, but it is as best as we can with the data available. We then get the following chart for the jury votes, normalized by the smallest amount of points:

<figure>
	<a href="/images/eurovision/jury-pop-votes.png"><img src="/images/eurovision/jury-pop-votes.png" alt="The jury vote weighted by country population."></a>
	<figcaption>The jury vote weighted by country population.</figcaption>
</figure>

Wow, is that Russia winning clearly over there? To remind you, although Russia has a large population of over 140 million, it cannot vote for itself. Also, France and Ukraine jump past Australia. There are also a lot of countries with very few points. Let's take a look at the televote graph, but now also weighted by the country population:

<figure>
	<a href="/images/eurovision/tele-pop-votes.png"><img src="/images/eurovision/tele-pop-votes.png" alt="The televotes weighted by country population."></a>
	<figcaption>The televotes weighted by country population.</figcaption>
</figure>

I think we have a clear winner here. Ukraine, Poland and France still get quite a few points, but Russia is the indisputable number one. Combining both jury and televotes gives the following:

<figure>
	<a href="/images/eurovision/total-pop-votes.png"><img src="/images/eurovision/total-pop-votes.png" alt="The combined jury votes and televotes weighted by country population."></a>
	<figcaption>The combined jury votes and televotes weighted by country population.</figcaption>
</figure>

Do you need more evidence that Russia is indeed the only rightful winner of the last Eurovision song contest? It is in my judgement that this scoring system much better reflects the true opinion of the average European person, more than taking an average of every European country.

To finish this post, I want to find out what country has the most 'capacity' to predict the final outcome of the voting, that is, what country reflects the average European opinion. Therefore, for every country, I calculate a simple \\(L_1\\) error function, which reflects how many points the country's vote differs from the final outcome:

\\[
Error_c = \sum\_{c' \in countries\setminus\\{c\\}} \left| given\_{c'} - final\_{c'} \right|
\\]

In this formula, the points are given as explained above. We do not take into account the country population as before, we just use the traditional scoring system. First check out only the jury votes; the graph below shows the total error for each country:

<figure>
	<a href="/images/eurovision/jury-err.png"><img src="/images/eurovision/jury-err.png" alt="The total error for the jury votes."></a>
	<figcaption>The total error for the jury votes.</figcaption>
</figure>

Israel scores best, with an error of 35 points; Czech Republic has an error of 86 points. So there is quite a difference between the countries. Note that a high error can also mean that the country votes tactically, by giving points to all countries which it thought would perform bad in the contest. Russia, Ukraine and France have quite a high error, and in the calculation I did not include the voting country! The average error is 59.8 points. Let's take a look at the televote errors:

<figure>
	<a href="/images/eurovision/tele-err.png"><img src="/images/eurovision/tele-err.png" alt="The total error for the televotes."></a>
	<figcaption>The total error for the televotes.</figcaption>
</figure>

Bulgaria is now the best predictor, with an error of only 24 points. Switzerland is at the tail with a 74 points error. The average error is 43.7 points, much lower than for the jury votes. This can mean different things. Do different juries tend to disagree more, and do the masses agree more? Do juries vote more tactically? Who knows... Combining the jury votes and televotes, we get the following picture:

<figure>
	<a href="/images/eurovision/total-err.png"><img src="/images/eurovision/total-err.png" alt="The total error for all votes."></a>
	<figcaption>The total error for the all votes.</figcaption>
</figure>

The winner is Israel with 32 points error, followed by Latvia with 34 points. At the tail we have Ireland with 78 points error, and Switzerland with 76 points error. The average error is 52.4 points. I also learn the my country, Belgium, is quite a bad predictor for the final result. Maybe that's why we never win the contest...



[^1]: [http://eurovisionworld.com/?odds=eurovision](http://eurovisionworld.com/?odds=eurovision){:target="_blank"}