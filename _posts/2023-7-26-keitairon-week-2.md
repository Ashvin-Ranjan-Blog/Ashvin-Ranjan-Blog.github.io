---
title: 'Keitairon Week 2'
excerpt_separator: '<!--more-->'
categories:
  - Linguistics
tags:
  - Linguistics
  - Languages
  - Japanese
---

Day 1:

After creating derivations the next thing to move on to was special verbs. Japanese has a small number of irregular verbs, and luckily all of them (except for 2) only have a few changes that need to be made. At first, my approach was to create special conjugations tables for each irregular verb, but then I realized that all of the irregular verbs follow either the Ichidan or Godan verb conjugations, with a few exceptions, and I could make special rules for those exceptions.<!--more-->

To test this out, I added two very simple irregular verbs: 問う and 呉れる. Adding in the special conjugation for 問う was fine, but for 呉れる, another problem arose. 呉れる and 暮れる are both written the same (くれる) however, the imperative form of 呉れる is 呉れ and the imperative form of 暮れろ. To fix this, I added a special marker to make sure that special rules only applied to the special verbs. I continued this to いる, though after this I realized I desperately needed a filter, as it is disallowed in the grammar to take the stem of いる, if one wants to take the stem of いる they have to instead use おる.

Finally, I added する, one of the more difficult verbs. It is one of the two which do not have small changes, with it requiring many additional special rules to get working. I was able to fit する into the Ichidan conjugation chart, however, I still needed a filter to ban it from being in potential form, because the potential form of する is できる, which is a wholly different word. Along with する, I added する verbs, which are nouns that can have する at the end to make them an action. These also followed the proper する conjugation charts.

Day 2

After the work of the previous day, I was set to quickly tackle the remaining irregular verbs, first was ある, which had a simple negative form change. However, this change brought up another issue that I had neglected for a bit. When a Keyoushi adjective is put in its negative form, it has ある appended to its adverb form, however, ある can only be in its negative form, to allow for this a filter would need to be made, so I put off creating that until I was able to create the filter.

After ある, the final and hardest verb remained: くる. like する, くる has nearly an entire separate chart to itself with different stems and endings depending on its inflection. After thinking, I was able to fit くる as a Godan verb, as it had two base stems き and こ for regular and negative respectively. However, I still had to deal with several annoying issues, such as the volitional form (こよう) and the imperative form (こい), but after those were finished all of the irregular verbs were (for the most part) handled.

Day 3:

Not much was done this day, I simply added 化する (to allow nouns to change into verbs) which used the する grammar I had already implemented, and fixed some missing conjugations with Ichidan verbs.

Day 4/5:

I mostly rested these days, I had been working hard the past week and a half and needed a break from the work that I was doing.

Day 6:

This day I worked on setting up a filter. At first, I thought I succeeded, the filter was blocking out incorrect grammar like the stem of いる, it was only later I realized that the filter was not blocking the correct things, it was blocking everything. I tried to look at XMOR and the SFST tutorial to see how filtering was handled, but could not find anything that could help me, so I put off filtering and worked on Noun conjugations.

Noun conjugations were simple, there are not too many suffixes and most of them cannot be stacked, as such I was simply able to reimplement the grammar used for the verb and adjectives to deal with nouns.

However, there was one more thing I attempted to do. I wanted to add て form combinations, as some verbs have special meanings when put after the て form of another verb. While creating these rules I stumbled upon two severe issues I missed. The first one was less serious, I had forgotten phrases. Phrases like ありがとう may be made more polite by adding どうも to the front and ございます (not to mention that ございます can also be in past tense). This was an easily fixable issue, however, and could be addressed later, the bigger issue was that of the derivation system. The derivation system was never made to be able to handle cyclic derivation, and now with two instances of verb-to-verb derivation, the whole system seemed like it was ready to collapse. Due to these issues, I put off adding て form combinations and instead moved to other ventures.

Day 7:

I wanted to make what I was doing into a web application, to achieve these goals I would have to do one of two things: either compile the `.lex` files to JavaScript, TypeScript, or WASM, or interpret the compiled `.a` files in JS, TS, or WASM. Both of these options seemed hard and the challenge got even more daunting when I looked at the SFST source code. To translate the C++ source code into TS I would have to rewrite over 1000 lines in TS, and my knowledge of C++ programming was minimal at best.

I was about to start rewriting when one of my friends gave me a useful idea: why not compile the C++ into WASM? I did some research and it turns out there is a relatively simple way to do this compilation and make a web page, so that is what I started on next. However, there was one final issue that arose, I needed to rewrite the original code to allow a JS interface to interact with it, and that meant changing the analyzer. I rewrote the code easily by referencing the source code, however, the code reads the system files to retrieve the data, and it was too invasive to attempt to rewrite the FST code to fix it. I did more reading on the compiler and it turns out I can create a faux file system for the compiled WASM code to read the file needed. With this, I compiled the C++ code. I have yet to test if it works, but I have high hopes for a web version of Keitairon that can be used easily by everyone.
