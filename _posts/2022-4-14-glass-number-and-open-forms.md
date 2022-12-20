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

This recursive formula generates a sequence of integers (given the starting values of $$1, 1$$) that have a ratio that approaches the value $$\varphi$$. $$\varphi$$ can be described in many ways, and it is known that

$$\varphi^2 - \varphi - 1 = 0$$

Which we can then use to state:

$$\varphi^2 - \varphi - 1 = 0$$

$$\varphi^2 = \varphi + 1$$

$$\varphi = \pm \sqrt{1 + \varphi}$$

$$\varphi = \pm \sqrt{1 \pm \sqrt{1 \pm \varphi}}$$

$$\varphi = \pm \sqrt{1 \pm \sqrt{1 \pm \sqrt{\cdots}}}$$

This is just the first in a set of many interesting open forms which can be described, here is another:

$$\varphi^2 - \varphi - 1 = 0$$

$$\varphi - 1 - \varphi^{-1} = 0$$

$$\varphi^{-1} = \varphi - 1$$

$$\varphi^{-1} = \varphi\left(1 - \varphi^{-1}\right)$$

$$\frac{\varphi^{-1}}{1 - \varphi^{-1}} = \varphi$$

$$\varphi = \varphi^{-1}(1 + \varphi^{-1} + \varphi^{-2} + \cdots)$$

$$\varphi = (\varphi^{-1} + \varphi^{-2} + \varphi^{-3} + \cdots)$$

$$\varphi = \sum_{n=1}^{\infty} \varphi^{-n}$$

We are able to go from the fraction to the infinite sum using the sum of an infinite geometric series forumua. The ratio here being $$\varphi^{-1}$$, given $$\varphi > 1$$, $$\varphi^{-1} < 1$$, allowing us to use that theorem.

## The Plastic Number and the Supergolden Ratio

Here we shall head into the more obscure territory. Firstly, the [Supergolden ratio](https://en.wikipedia.org/wiki/Supergolden_ratio). The Supergolden Ratio is the ratio found through the Sequence $$F_n = F_{n-1} + F_{n-3}$$. It can be expressed through the polynomial $$\psi^3 - \psi^2 - 1 = 0$$ and using similar logic to $$\varphi$$, we are able to prove that it has this open form[^1]:

$$\psi = \sum_{n=2}^{\infty} \psi^{-n}$$

The introduction to the Supergolden ratio, while brief here, is important to the ideas of the Glass number.

The second value we shall take a look at is the [Plastic number](https://en.wikipedia.org/wiki/Plastic_number). The Plastic number is defined by $$\rho^3 - \rho - 1 = 0$$, and is actually the ratio approximated by the sequence $$F_n = F_{n-1} + F_{n-5}$$[^2]. From this we are able to quickly see that we can generate two open forms:

$$\rho^3 = \rho + 1$$

$$\rho = \sqrt[3]{\rho + 1}$$

$$\rho = \sqrt[3]{1 + \sqrt[3]{1 + \sqrt[3]{\cdots}}}$$

And

$$\rho = \rho^3 - 1$$

$$\rho = (\rho^3 - 1)^3 - 1$$

$$\rho = ((((\cdots)^3 - 1)^3 - 1)^3 - 1)^3 - 1$$

Note that the logic for the second open form may be applied to $$\varphi$$ as well.

[^1]: Proving this is left as an exercise to the reader
[^2]: Proof of this is able to be found in another, more complex Linear Recurrence paper [here](https://www.overleaf.com/read/qbjzkpmnnhjr).
