---
title: 'Can We Procedurally Generate Kanji?'
excerpt_separator: '<!--more-->'
last_modified_at: 2023-7-19T12:30:00-08:00
categories:
  - Linguistics
tags:
  - Linguistics
  - Languages
  - Japanese
---

I was recently thinking about how one could programmatically generate an infinite amount of Kanji. Kanji itself is made out of radicals, and it may be trivial to isolate radicals and use a random selection of them to generate a new kanji.

<!--more-->

This idea seems simple and innocuous, however, an issue arises when actually isolating these radicals. For example: look at the radical 五, now look at how it is used in 語. Notice that it is scaled down, the issue here is that it is scaled down, but the width of the lines has stayed the same. One might think this is easily emulated, after all, the SVG file format (which governs most fonts) has stroke width as an option for a line. The issue is, every single font does not use lines with stroke widths to create their characters, they outline them, so scaleable radicals for now is a thing which, without hours of manual labor, I cannot do. I will continue looking for other ways to do this, but for now, it's best to keep this in my drafts.

UPDATE [7/19/2023]: Turns out that there is a way for these radicals to be isolated. I was recently, as part of another project, looking through the "About" page of [jisho.org](https://jisho.org/about) and looked through their open source projects, where I came upon [kanjivg2svg](https://github.com/Kimtaro/kanjivg2svg), this uses the [KanjiVG](http://kanjivg.tagaini.net/) project to create specially-formatted SVG files of kanji. Since these go off of strokes, these may provide ways to generate radicals that use strokes instead of outlines, completely fixing the issue. I will have to do more research into this but I hope that soon I will be able to make another post that shows the completion of this project.
