---
title: 'Finding the Size of an LRU Cache Fast'
excerpt_separator: '<!--more-->'
last_modified_at: 2025-5-01T11:08:00-08:00
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

The column on the right does not immediately seem trivial to understand, so we can instead leverage resources such as _The Online Encyclopedia of Integer Sequences_. Given the amount of research already done on sequences, it is likely that what we are looking for is there, and it is. Sequence [A025480](https://oeis.org/A025480) matches what we are looking for.

We can see that it exactly matches the recursive definition. A way to interpret how the values in the right column are gotten is by considering the table starting as so:

| Load | Check |
| :--: | :---: |
|  0   |   -   |
|  1   |   -   |
|  2   |   -   |
|  3   |   -   |
|  4   |   -   |
|  5   |   -   |
|  6   |   -   |
|  7   |   -   |
| ...  |  ...  |

To fill out each item in check, the table is read left to right, top to bottom. This means that the first value in the check column is 0 since we read the first value in the table as 0. Then when filling out the second item in the check column we write 0 again, since we are reading the 0 we just wrote in the check column. If we continue like this we get 1, then 0, then 2, then 1, and so on. We can see that for every even-index value we read from the left column, and for every odd-number value we read from the right column. Looking at the recursive definition we see that $$a(2n)=n$$ matches the fact that on even indices we read from the right column, and $$a(2n+1)=a(n)$$ matches the fact that on odd indices we read from the left column.

This is important when using the explicit formula for $$a(n)$$, as it can be calculated directly by expressing $$n+1$$ in binary and then removing all trailing zeros and shifting right one more time[^1]. This runs in constant time in regards to cache size, so we can now write the following pseudocode solution:

```
index = 0
while true {
 cache.access(index)
 if (!cache.access(index >> (get_trailing_zeros(index + 1) + 1))) {
 return index - 1
 }
}
```

And looking at this we can see that the solution runs in linear time.

## But what about getting the trailing zeros

At this point, one may ask about the suspicious `get_trailing_zeros` function I included above. To comprehensively prove that the method above is $$\Theta(n)$$, I can prove that `get_trailing_zeros` can be implemented in $$\Theta(1)$$ time complexity.

We can see for languages with fixed integer sizes, that `get_trailing_zeros` can be implemented with a simple, constant time loop. See the below C code:

```c
int get_trailing_zeros(int i) {
  int out = 0;
  for (int j = 0; j < 32; j++) {
    if (!(i & 1)) {
      out++;
    }
    i >>= 1;
  }
  return out;
}
```

However, not all languages have fixed integer sizes. For example, Python dynamically resizes integers based on size. One may think that this would make `get_trailing_zeros` a $$\Theta(\log{n})$$ operation. However, we can take another trick from [A025480](https://oeis.org/A025480) (with a bit of adaptation for 0-indexing) and do the following:

```py
def get_trailing_zeros(n): return (~n&(n-1)).bit_length()
```

We can see that this is $$\Theta(1)$$[^2]. But how does this work? The reason comes from two intuitions we can make. The first is that when a number is subtracted by 1 in binary, all trailing 0s turn to 1s, the rightmost 1 turns to a 0, and all other digits are unaffected. The other is that when a number is inverted, all trailing 0s turn to 1s, and all bits are exact opposites of the original number. Because both of these operations turn all trailing 0s to 1s, when they are combined with a bitwise and operation all trailing 0s are left as trailing 1s in the final number, furthermore all numbers after and including the first 1 are set to 0 as they are either 0 (as is the case with the first 1), or they are the exact opposite of the value in the binary inverse. This means what is remaining is simply a number with 1s equal to the number of trailing 0s in the original number. Getting the bit length of that integer then gives the number of trailing 0s in the original number.

[^1]: From "Fractal Sequences and Restricted Nim" by Professor Lionel Levine.
[^2]: There is the question of what the time complexity of `int.bit_length` actually is but we can see through [this StackOverflow post](https://stackoverflow.com/questions/58461990/worst-case-time-complexity-of-pythons-int-bit-length) that it is $$\Theta(1)$$.
