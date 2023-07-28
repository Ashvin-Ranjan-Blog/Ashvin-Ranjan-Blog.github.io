---
title: 'Making Keitairon, a Japanese Morphological Analyzer'
excerpt_separator: '<!--more-->'
last_modified_at: 2023-7-22T17:30:00-08:00
categories:
  - Linguistics
tags:
  - Linguistics
  - Languages
  - Japanese
---

Day 1:

Today I have decided to start a new project called Keitairon. The name comes from the Japanese word for morphology (形態論) and my main goal is to create a program that can describe the different morphologies of a verb in higher detail. This comes from the fact that [jisho.org], while it can recognize some of the morphologies, struggles with others. Along with this, I hope that this will be able to be reversed and that all possible conjugations of a verb can be shown. (Maybe this can also extend to nouns, like with 無し).

<!--more-->

I started by looking at what [jisho.org] uses to handle their analysis, which led me to [MeCab](https://taku910.github.io/mecab/). I had noticed the term "Morphological Analyzer" and that took me down a long rabbit hole of research and confusion:

I first started by looking at [this tutorial for foma](https://fomafst.github.io/morphtut.html) which gave me the first lead: "Finite-State Transducers". The tutorial talked about turning tags into inflections instead of what I wanted, which was inflections into tags, so I gave up and looked for something else that could help me along my journey.

Eventually, I read about Hidden Markov Models, and being that I had an amount of experience with Markov chains before (given my old text generation antics in Freshman year). I decided to take a look at it. At first, the notation of it all scared me, but then I found a wonderful tutorial series by a YouTube channel called Normalized Nerd which explained to me how it all worked. Though unfortunately I still was not closer to seeing how I could use these to process information. I attempted to look at other projects which did similar things to what I was doing but to no avail.

This is when I came across a [presentation](https://www.andrew.cmu.edu/user/ko/downloads/11-411-Natural-Language-Processing-Slides.pdf) for a college course on Natural Language Processing. In there it talked about what FSTs were and why they were used, and it talked about the inversion operator with an FST. I thought about the tutorial from earlier and how I wished I could invert it, I eventually went back to it and finally read to the bottom and there it was:

> This is indeed one of the main reasons to use finite-state transducers for morphological analysis: the fact that we can describe the word-formation rules in the direction of generation, but use the final FST in the other direction.

Upon reading this I started going straight into attempting to program something for Japanese, but I soon hit a brick wall. Conjugation in Japanese not only adds to the original word but changes the base word as well, and dealing with that was not going to be as easy. So, armed with my new information, I continued the search for better ways of doing what I wanted, and eventually, I stumbled upon [mlmorph](https://github.com/smc/mlmorph/tree/master), a program to process Malayalam in SFST.

Malayalam is an agglutinative language, which means that it continues to add onto the base word to create meaning (think Turkish). This is somewhat similar to (although more aggressive than) Japanese, and I was able to use this as a way to introduce myself to SFST, which I am currently using to write Keitairon. I am currently working on the tutorial, which defines a simple English analyzer, to then be able to understand the program well enough to work on a Japanese analyzer.

I hope that in the future after all of this is done, I can maybe work on documenting some lesser looked-at languages, similar to what Santhosh Thottingal has done with Malayalam. I have a friend who speaks Finlandssvenska (Finland Swedish) and I hope that I will be able to work with her to be able to document it.

Day 2&3:

Both of these days are very similar, so it is best to instead summarize them as one. I was thankfully able to get confirmation from Professor Dan Jurafsky that I was on the right path with SFST, and I eventually continued my work on figuring it all out. I eventually got stems working for Ichidan Verbs (The most simple type of verbs which only need one step to conjugate), however, I ran into a strange issue where in the generative direction it will work as intended, but in the analysis direction, it would not recognize anything. Eventually, I decided to use some community tools to visualize the FST and found my issue. I was using `fst-compiler` not `fst-compiler-utf8`, unfortunately nowhere in the tutorial did it mention this discrepancy (most likely because the tutorial was for an English analyzer) but in the end, I was able to get that fixed.

I was able to continue on Godan Verbs (Verbs with a bit more conjugation rules) and got stuck at Past-tense, getting stuck in cycles of infinite analysis until I realized that I was using `||` instead of `&` to combine my inflection rules (I really should start reading these documents a bit more). In the end, I was still unable to get past tense working (Because it relies on one of the most inconsistent parts of Japanese conjugation), and I will have to look into it more soon.

My main challenge which remains is the fact that Japanese agglutinative. I cannot work on several conjugations of Japanese simply because they end with the verb turning into a noun, an adjective, or simply another verb, and I will have to do more research and how to allow that with SFST. There is hope on the horizon though, while the list on SFST's website was broken I was still able to track down the paper for [TRMOR](https://journals.tubitak.gov.tr/cgi/viewcontent.cgi?article=1547&context=elektrik), a Turkish FST written in SFST. I hope this will give me some useful pointers on where to go in the coming days.

Day 4-7:

After reading about TRMOR, I was still unable to find any useful information that could help me in my search for derivation. Seeing as I had reached complete dead-ends, I decided to contact [Professor Helmut Schmid](https://www.cis.uni-muenchen.de/~schmid/), the creator of SFST. After contacting him I was able to receive a pointer to an example of SFST, [XMOR](https://github.com/santhoshtr/sfst/tree/master/data/XMOR). In XMOR derivations are handled by creating stems and adding suffixes onto them. I have already done this with helper verbs, but after looking at the code, the way to implement derivation clicked with me suddenly. I would need to separate all conjugations which are derivations and repetitively apply them before the other conjugations could be applied to the verbs. I did this and sure enough, I was able to get derivations working with all types of verbs.

However, everything is not completely perfect, I have still yet to filter out invalid conjugations, such as Passive-Casusative form, but XMOR also has some examples of that so I am not too worried about invalid conjugations at present. The current main issue is the special verbs that Japanese has. Luckily for me, there are not too many of them, but some of the verbs (specifically する and くる) present so many different conjugations that I am worried I will have to create separate conjugation charts for them. This is made even worse by the fact that I cannot deal with some noun conjugations and する verbs until I have する figured out. Along with する, I attempted to try to add Kanji to the list of possible symbols for the program, however, the compilation of the program was very slow, and the more complex this program gets, the more technical baggage I will have to overcome with the addition of kanji, especially because some grammar points have optional kanji which may be used. But for now, I will just stick to kana and hope that kanji will not be too much of a pain to implement.
