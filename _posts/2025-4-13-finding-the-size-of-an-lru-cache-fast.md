---
title: 'Finding the Size of an LRU Cache Fast'
excerpt_separator: '<!--more-->'
last_modified_at: 2025-4-13T23:20:00-08:00
categories:
  - Computer Science
tags:
  - Computer Science
---

Consider the question "Given an empty least-recently-used cache, find the size of the cache if you can to tell when a search is a miss?" The straightforward solution to this is easy, load a new cache block, and then look through all of the old cache blocks to make sure that none of them unloaded. Repeat that until you find one of the previous blocks unloaded, and there you go. Assuming that checking the cache is in constant time, that solution is $$\Theta(n^2)$$. However, we don't have to look through all of the previous blocks to check if one deloaded, right? Is a $$\Theta(n)$$ solution possible?

<!--more-->

## The Answer

The answer is yes, but it is not as simple as it initially seems. The first idea for optimization which may come to mind is to recheck the first block after every new access, instead of checking all of the previous blocks. This intuition is a good start, but checking if a block is loaded or not changes whether or not it will be deloaded, as such, that idea will not work when the cache size is greater than 1:

We do know that if we access the items in the same order they will deload in the same order, that way we can start to write out a sequence of loads and checks and see if any pattern emerges:

| Load | Check |
| :--: | :---: |
|  0   |   0   |
|  1   |   0   |
|  2   |   1   |
|  3   |   0   |
|  4   |   2   |
|  5   |   1   |
|  6   |   3   |
|  7   |   0   |
| ...  |  ...  |

The column on the right does not immediately seem trivial to understand, so we can instead leverage resources such as the _The Online Encyclopedia of Integer Sequences_. Given the amount of research already done on sequences, it is likely that what we are looking for is there, and it is. Sequence [A025480](https://oeis.org/A025480) matches what we are looking for.

We can see that it is an exact match from the recursive definition. The way the table was created was that the left column simply counts up, which can be considered $$n$$, and the right column is the $$n^{\text{th}}$$ item in the table when read left-to-right, top-to-bottom. Looking at the recursive definition we see that $$a(2n)=n$$, which matches the left column of the table, and $$a(2n+1)=a(n)$$, which matches the right column of the table. $$a(n)$$ can be calculated directly by expressing $$n$$ in binary and then removing all trailing zeros and shifting right one more time[^1]. This runs in constant time in regards to cache size, so we can now write the following pseudocode solution:

```
index = 0
while true {
  cache.access(index)
  if (!cache.access(index >> (get_trailing_zeros(index) + 1))) {
    return index - 1
  }
}
```

And looking at this we can see that the solution runs in linear time.

[^1]: From "Fractal Sequences and Restricted Nim" by Professor Lionel Levine.
