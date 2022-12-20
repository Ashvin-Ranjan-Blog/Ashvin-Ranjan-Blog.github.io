---
title: 'DRAFT: The Glass Number and Interesting Open Forms'
excerpt_separator: '<!--more-->'
categories:
  - Drafts
tags:
  - Mathematics
  - Linear Recurrences
  - Drafts
---

For a long time, a friend and I have been investigating linear recurrences, and in this blog post, I will briefly detail extracts from our research. You can find a complete paper [here](https://www.overleaf.com/read/rxxktbhpdrdt).

<!--more-->

## The Fibonacci Sequence and Phi

Let us begin at a point of reference that many will have heard of. The Fibonacci sequence is defined by the recrursive formula:
$$F_n = F_{n-1} + F_{n-2}$$
This recursive formula generates a sequence of integers (given the starting values of $1, 1$) that have a ratio which approaches the value $\phi$. $\phi$ can be described in may ways, and it is known that
$$\phi^2 - \phi - 1 = 0$$
Which we can then use to state
\begin{align}
\phi^2 &= \phi + 1 \\
\phi &= \pm \sqrt{1 + \phi} \\
\phi &= \pm \sqrt{1 \pm \sqrt{1 + phi}} \\
\phi &= \pm \sqrt{1 \pm \sqrt{1 + \sqrt{\cdots}}} \\
\end{align}
