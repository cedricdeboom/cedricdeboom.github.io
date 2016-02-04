---
layout: post
title: "The probability of overlap"
excerpt: "A combinatorial experiment has gotten out of hand"
categories: blog
tags: [mathematics, combinatorics, language]
comments: true
share: true
image:
  feature: overlap-header.jpg
  credit: House of Jo
---

Oh dear, what started yesterday as a simple combinatorial puzzle, turned out not to be that straightforward as we first thought. In other words, it's gotten completely out of hand. Are you ready to witness how quickly simple things can turn into 'chaos'?

For my [research on short-text similarity](http://arxiv.org/abs/1512.00765){:target="_blank"}, I was interested in the following question:

> Given a certain vocabulary or dictionary of words, create two sentences out of it at random. Now, what is the probability then that at least one of the words in these two sentences is the same?

A fairly simple question, no? I want to know whether, by uttering two completely different sentences on completely different subjects, it is possible to use at least one word in common. Think of it as follows: we have a set of all distinct objects and we randomly take items from it (we are allowed to take the same item more than once). Do this a second time, and verify if you have taken at least one item you had taken the first time.

Now, you probably want to correct me on that analogy. The probability of taking an item from a set is known to be uniformly distributed. On the other hand, choosing a word out of a dictionary to use logically in a sentence [is known to be distributed in a zipfian way](http://www.youtube.com/watch?v=fCn8zs912OE){:target="_blank"}[^1]. That is, words such as 'the' and 'is' are more likely to be chosen than e.g. 'discontinuation'. However, to start things off more easily, I made abstraction of the zipf distribution of word frequencies, and focused first on solving the problem in a uniform setting.


<figure>
	<a href="/images/overlap/zipf.png"><img src="/images/overlap/zipf.png" alt="The Zipf distribution" width="500px"></a>
	<figcaption>Illustration of the Zipf distribution. Image courtesy: Wikipedia.</figcaption>
</figure>



## The Good

Let's start with the simple examples. Assume that we have a vocabulary \\(V\\) of \\(n\\) words, and for now let's even assume that \\(n=5\\). We now create two sentences \\(z_1\\) and \\(z_2\\) using words from \\(V\\). We take both sentences of equal length \\(m\\) in order not to overly complicate our discussion. Before we go on, note that the probability that at least one word overlaps in the two sentences is equal to one minus the probability that no words overlap. So actually we are calculating the no-overlap probability

Ok, say \\(m=1\\). Then we have five choices for the word in the first sentence, and four choices for the word in the second sentence if we do not want any overlap. In that case:

\\[
P(overlap) = 1 - \frac{5\cdot 4}{5^2} = 0.2.
\\]

So far, so good. Let's take \\(m=2\\). Now we have more options. Either we have the same two words in the first sentence (and four choices for the words in the second sentence), or we have two different words (and three choices for the words in the second sentence). Counting both options, we get:

\\[
P(overlap) = 1 - \frac{5\cdot 1 \cdot 4 \cdot 4 + 5\cdot 4 \cdot 3 \cdot 3}{5^4} = 0.584.
\\]

Ok, continue, now \\(m=3\\). Again, more options. Either three unique words in \\(z_1\\), two unique words or only one. In the case of two unique words, however, we have to count the different orderings of the words in the sentence; if we are careful not to count double, then the extra multiplicative factor is \\(\binom 32\\), which I have verified numerically. The probability of overlap is thus:

\\[
P(overlap) = 1 - \frac{5\cdot 4 \cdot 3 \cdot 2 \cdot 2 \cdot 2 + \binom 32 5\cdot 4 \cdot 3 \cdot 3 \cdot 3 + 5\cdot 4 \cdot 4 \cdot 4}{5^6} = 0.741.
\\]

In fact, we can also write this as follows (since \\(\binom 33\\) is equal to 1):

\\[
P(overlap) = 1 - \frac{5\cdot 4 \cdot 3 \cdot 2 \cdot 2 \cdot 2 + \binom 32 5\cdot 4 \cdot 3 \cdot 3 \cdot 3 + \binom 33 5\cdot 4 \cdot 4 \cdot 4}{5^6} = 0.741.
\\]

The probability is quickly increasing, as we would indeed expect. By now you could say that you see the pattern here. By increasing the sentence length with one, we get an extra option we have to consider. It's not that simple however...


## The Bad

To illustrate the extra complexity, let's assume now that \\(m=4\\). Again, as before, we have four options now: four unique words in \\(z_1\\), three unique words, two unique ones, and all words the same. However, there is a fifth option now, namely 2 + 2 unique words (such as "a b b a" or "c c b b"). In the following we will only list the number of possibilities for the first sentence, otherwise the notation will become complicated.

In option 1 we have four unique words in the first sentence, which gives us the following factor in the numerator: \\(5\cdot 4\cdot 3\cdot 2\\). In option 2 we have one word which occurs double, so we have: \\(\binom 42 5\cdot 4\cdot 3\\). In option 3 there is one word which occurs triple: \\(\binom 43 5\cdot 4\\). In option 4 all words are equal in the first sentence: \\(\binom 44 5\\). And then, finally, in option 5 we have two words which both occur double: \\(\frac{1}{2}\binom 42 5\cdot \binom 22 4\\). We have to divide by two since otherwise we would be counting all sentences twice.

So, not that straightforward anymore. For a sentence of length 5 for example we first need to calculate how the number 5 can be composed out of smaller numbers, in this case: \\((5)\\); \\((4,1)\\); \\((3,2)\\); \\((3,1,1)\\); \\((2,2,1)\\); \\((2,1,1,1)\\); \\((1,1,1,1,1)\\). Such decompositions are called [_'partitions'_](https://en.wikipedia.org/wiki/Partition_(number_theory)){:target="_blank"} in number theory. An algorithm to find such partitions can easily be implemented using recursion.


## The Ugly

We want to put the overlap probability into a closed-form formula. For this purpose, we have to define several quantities based on number partitions. Let's call \\(\mathcal{P}_n\\) the sequence of partitions of the number \\(n\\). The \\(i\\)'th element \\(\mathcal{P}\_{n,i}\\) is a multiset (i.e. a set in which elements are allowed to occur more than once) which represents a single partition. Then \\(\mathbb{1}(a, \mathcal{P}\_{n,i})\\) is the multiplicity of the number \\(a\\) in the multiset \\(\mathcal{P}\_{n,i}\\). Finally, if we denote a multiset by \\(p\\), then we denote the corresponding set by \\(p'\\); that is, if an element occurs multiple times in \\(p\\), we only keep one of them in \\(p'\\). In the following we also assume that \\(n\geq m\\), which is always the case in practical language settings.

For option 1, which is all different words in the first sentence, we get the following factor in the numerator of the probability, which is straightforward:

\\[
\frac{n!}{(m-n)!}(m-n)^m
\\]

For the other options, we need to consider the partitions of \\(m\\). We get the following factor in the numerator of the probability:

\\[
\sum_{p\in \mathcal{P}\_m\setminus (1)^m} \frac{n!}{(|p|-1)!}(|p|-1)^m
\\]

However, every factor needs an appropriate binomial coefficient and correction factor, so that the above becomes a little more complicated:

\\[
\sum\_{p\in \mathcal{P}\_m\setminus (1)^m} \frac{n!}{(|p|-1)!}(|p|-1)^m  \prod\_{k\in p', k>1} \frac{1}{\mathbb{1}(k, p)!} \prod\_{i=1}^{|p|} \binom{m - \sum\_{j=1}^{i-1} p\_j}{p_i}
\\]

Phew, that's heavy. The probability of overlap now becomes, in all its glory:

\\[
\begin{align}
P(overlap) = &1 - \frac{1}{n^{2m}} \left( \sum\_{p\in \mathcal{P}\_m\setminus (1)^m} \frac{n!}{(|p|-1)!}(|p|-1)^m + \\\\\sum\_{p\in \mathcal{P}\_m\setminus (1)^m} \frac{n!}{(|p|-1)!}(|p|-1)^m  \prod\_{k\in p', k>1} \frac{1}{\mathbb{1}(k, p)!} \prod\_{i=1}^{|p|} \binom{m - \sum\_{j=1}^{i-1} p\_j}{p_i} \right)
\end{align}
\\]

This can easily be extended to two sentences of different lengths. Incorporating a zipfian distribution however will not be that easy... I do not immediately see how I can further simplify the probability expression above, although I am not a mathematician. I have looked at Stirling numbers, but they don't seem to help me any further. The comments section below is open for any suggestions!


[^1]: A very cool website demonstrating this principle is [www.wordcount.org](http://www.wordcount.org){:target="_blank"}.