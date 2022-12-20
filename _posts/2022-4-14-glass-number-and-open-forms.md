---
title: 'DRAFT: The Glass Number and Interesting Open Forms'
hidden: true
last_modified_at:
---

For a long time, a friend and I have been investigating linear recurrences, and in this blog post, I will briefly detail extracts from our research. You can find a complete paper [here](https://www.overleaf.com/read/rxxktbhpdrdt).

<!--more-->

## The Fibonacci Sequence and Phi

Let us begin at a point of reference that many will have heard of. The Fibonacci sequence is defined by the recrursive formula:

$$F_n = F_{n-1} + F_{n-2}$$

This recursive formula generates a sequence of integers (given the starting values of $$1, 1$$) that have a ratio that approaches the value $$\phi$$. $$\phi$$ can be described in many ways, and it is known that

$$\phi^2 - \phi - 1 = 0$$

Which we can then use to state:

$$\phi^2 - \phi - 1 = 0$$

$$\phi^2 = \phi + 1$$

$$\phi = \pm \sqrt{1 + \phi}$$

$$\phi = \pm \sqrt{1 \pm \sqrt{1 \pm phi}}$$

$$\phi = \pm \sqrt{1 \pm \sqrt{1 \pm \sqrt{\cdots}}}$$

This is just the first in a set of many interesting open forms which can be described, here is another:

$$\phi^2 - \phi - 1 = 0$$

$$\phi - 1 - \phi^{-1} = 0$$

$$\phi^{-1} = \phi - 1$$

$$\phi^{-1} = \phi\left(1 - \phi^{-1}\right)$$

$$\frac{\phi^{-1}}{1 - \phi^{-1}} = \phi$$

$$\phi = \phi^{-1}(1 + \phi^{-1} + \phi^{-2} + \cdots)$$

$$\phi = (\phi^{-1} + \phi^{-2} + \phi^{-3} + \cdots)$$

$$\phi = \sum_{n-1}^{\infty} \phi^{-n}$$

We are able to go from the fraction to the infinite sum using the sum of an infinite geometric series forumua. The ratio here being $$\phi^{-1}$$, given $$\phi > 1$$, $$\phi^{-1} < 1$$, allowing us to use that theorem.
