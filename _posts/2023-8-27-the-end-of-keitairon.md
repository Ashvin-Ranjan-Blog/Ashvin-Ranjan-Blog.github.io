---
title: 'The End of Keitairon (For Now)'
excerpt_separator: '<!--more-->'
categories:
  - Linguistics
tags:
  - Linguistics
  - Languages
  - Japanese
---

Over the past 3 weeks, I've been with my relatives (both my cousins and my aunts, though my aunts are technically younger than me) so I have not been able to work on Keitairon that much, but that hasn't stopped some more important developments from occurring.<!--more-->

### Talking With a Professor

I was finally able to set up a meeting with [Professor Noriko Nagata \| 永田憲子教授](https://www.usfca.edu/faculty/noriko-nagata) and we were able to discuss not only the program but what I was using to learn Japanese and my future with learning it. I was really surprised because while I mostly talked with her in English regarding the program (I do not know that many specific Japanese words for programming), I was able to communicate somewhat fluently with her when talking about my personal life, though I did consistently have issues with using plain language instead of polite language, a habit I picked up from mostly talking with people online.

Professor Nagata stated that she had made a similar program before during her studies in Java. However, as opposed to Keitairon, this broke apart and corrected full sentences (similar to something like MeCab) as opposed to handling only morphology, though it did handle some. She also stated that this would be quite useful as a webpage to be able to help Japanese learners understand Japanese verbs better, not only analyzing but generating different forms. Furthermore, she also gave some advice specifically on derivation, stating that instead of allowing for verb derivations to be stacked one after another, I should just make a new Causative-Passive form and only apply one derivation, which would not only make everything a lot more simple, but it would also stop the need for Passive-Causative form filtering.

After talking with her about the program I also talked with her about learning Japanese. This part was mostly spoken in Japanese, but there were occasional parts where I ended up talking in English, either because I had a hard time processing what Professor Nagata said, or because I did not know how to phrase what I wanted to say in Japanese. Either way, we had a good discussion about where I was going to college, and I told her that I wanted to apply to Waseda University to help with my Japanese learning, we talked about the utility of that and also the quality of Japanese education in other colleges in the US.

### Updates to Keitairon

There were two major updates during this time. Firstly, I was able to get filters working. All of my earlier attempts at filters consistently placed the filtering functions in the wrong spot and filtered either nothing or everything, however this time I was able to place the filter properly (after all morphologies were applied but before stemmation and other rules came in) to allow for filtering of improper conjugations. The only issue with this is that now the filtering is a lot more complex, as I can just filter for `word<morphology>`, I have to filter for `word<stem>suffix`, which I then have to check against other things. While it is a pain, I have been able to filter out some of the current issues such as no ぬ or ず conjugation with ある and no stem of いる, though there are a lot more rules to implement.

Secondly, as foreshadowed in the previous section, derivation was reworked. Not only was verb derivation changed to only allow one per verb (adding Causative-Passive as its own derivation), but I also am working on making Keyoushi adjectives use ある as the proper way of negation, allowing for only the negative, polite negative, and polite-past negative of ある to be taken, though as of writing this blog it is still unfortunately broken. I hope to fix it in the coming days and push the new changes to derivation to allow for a more accurate way of interpreting verbs and adjectives with the program.

### The Future

I do hope to work on this in the future, however, my school is starting this week and with college applications and IBDP I am not sure if I will be able to sustain the pace of development that I have had. However, if you want to contribute, feel free to open a PR [here](https://github.com/Ashvin-Ranjan/Keitairon) and take a look at this list of issues:

- Finish all of the filtering
  - Remove the potential form of する
  - Deal with える and うる (Helper verbs)
  - Deal with short causative form if it ever comes up
- Add short causative form
  - This may not be useful since I don't think it's that widely used, though it is still important to support it
- Add in way more noun conjugations
  - Also allow support for prefixes
  - I am not sure how many are there, though I know for a fact that what I have right now is not it
- Actually scrape JMEDict to get all of the words and categorize them properly
- Look for any verb conjugations I am missing
  - The most glaring one is ずる verbs (ex: 感ずる)
  - I am not sure what other ones I am missing, but I also don't yet support archaic verb endings
- Clean up the code
  - The derivation code is an absolute nightmare and it could be so, so much better
- Add in て form "conjugations"
  - Specifically, things that have special meanings, such as ～ている
- Add in phrases
  - Phrases should also have some sort of conjugation:
    - ありがとう
    - ありがとうございます（ありがとうございました）
    - どうもありがとうございます（どうもありがとうございました）
- Add in Japanese documentation
  - This applies not only to the markdown files in the project but to the comments too.

After this blog post goes up I will push all of my changes, though adjectives will be broken at this point so there may be changes coming soon. If you want to help with this project I would be extremely happy as I don't know if I have the time to continue working on this until 2024.
