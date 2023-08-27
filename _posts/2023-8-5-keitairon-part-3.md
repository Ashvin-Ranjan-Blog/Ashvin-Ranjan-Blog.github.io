---
title: 'Keitairon Week 3'
excerpt_separator: '<!--more-->'
categories:
  - Linguistics
tags:
  - Linguistics
  - Languages
  - Japanese
---

Instead of doing a day-to-day breakdown, this week was mostly two main challenges: creating a website and kanji. As such I will break it down like so.<!--more-->

### Creating a Website

To be able to run the code on a website I would either need to rewrite all of the code in TypeScript (or JS) or, upon the suggestion of a friend, I could use `emscripten` to compile the existing C++ code to WASM, which would allow me to run it online without going through the effort of translating the language.

So I started on `emscripten` using online tutorials to slowly work my way through compiling a simple C++ program. Once I had done that I started on the code which I pull from [SFST on GitHub](https://github.com/santhoshtr/sfst/). The main issue was that SFST was designed to read files, and initially, I was worried that everything would break due to that. However, I was able to find that you could [package files](https://emscripten.org/docs/porting/files/packaging_files.html) with `emscripten`. However, that was not the end of my issues. I have never really worked with C++ in any large capacity, and whenever I have tried I have found the experience so frustrating I never continued to use it for that project. But in this case, I didn't have any choice whether to use it or not. From this one of the biggest struggles started: memory errors.

My first real memory error popped up after I changed the code a little to work with a web format. It provided a `main` function to initialize the transducer, then an `analyze` and `generate` function to utilize it. The main issue was two-pronged. Firstly, the way that I was assigning the transducer in `main` caused a memory error, however, because these never gave a line number I would have to manually comment out lines to see if the issue would fix itself. Eventually, I tracked the issue down to a specific function call. I thought that this would be the end of the issue, however, after some investigation, it was not the function call that was causing the problem (even though commenting it out fixed the error, though removed the whole functionality of the transducer), it was the assignment of the transducer to a static variable which was causing problems. Secondly, after making some small changes to how the program was compiled to fix issues with function names being obfuscated it turns out that the newly compiled code did not run `main`, it run the `_start` entrypoint, which (while it should) did not include `main`, causing many issues.

However, `main` was not the end of the issues, it turns out that the code that I had been using was out of date with the version of the code which I used to compile the transducer, so I had to go and update the code, which created a much more severe problem, which also included more memory errors. After I downloaded and replaced all the code from the SFST website instead of GitHub, I found that the old function to get the output as a vector of strings was gone, you could only output the analysis or generated data to a file (usually `stdout`). The main issue with this was one I noted earlier with `emscripten`. I am not sure why, maybe it is an issue with using the `C` namespace or it's just something with `emscripten`, but attempting to write `stdout` or `cout` causes an instant memory error. This error does not occur when I compile with `clang++`, so it appears to be `emscripten` specific, and I am not sure exactly why it occurs, all I know is that due to this I am unable to finish making this a website at this time, even if I found a way to capture the console output in JavaScript to get what I am looking for.

### Kanji

An essential part of Japanese is kanji, and to be remotely useful I had to add kanji support for Keitairon. The main issue was special verbs, to handle this I was able to change the special case handling code to have two rules, one for kana and one for kanji (though for some like いる I was able to mess with the existing rules to get it to work). The main challenge was dealing with くる and 来る, the main challenge is that while くる is easily classified as a Godan verb, 来る acts more like an Ichidan verb, so the verb type tag on the end will depend on whether the verb uses kanji or not, hopefully, though any program that uses this will most likely have something to handle the fact that it is a special verb as that is part of the output (Though there would need to be some handling for anything that uses tags to generate instead of analyze).

The other main challenge with kanji I mentioned before is the brutal compilation times, as they have gone from quite quick to extremely slow. Even if kanji is not in any rules, by being in the alphabet the compilation time slows to a crawl, from this, it might be best to use some sort of makefile routine to analyze the kanji used in the lexicon files to be able to isolate the characters which are recognized to reduce the compilation time (hopefully) significantly, though that is an optimization for another time as makefiles are a topic I am not yet education in fully.

### The Future

I would love to work on this in the future but it seems like for now I will have to leave this as is because for the next week, I have to spend some time working on college work, and then I have to prepare for retaking the SAT. I have contacted a Professor ([Professor Noriko Nagata \| 永田憲子教授](https://www.usfca.edu/faculty/noriko-nagata)) and I hope to show her the program and get some feedback soon by someone who has deep knowledge in the subject. I am very hopeful for the future of this project though development may slow in the coming time.
