@def hascode = true

# Maximum Ribbon Cut

\blurb{We'll cover the following}

\lineskip

\toc

## Introduction

\ques{We are given a ribbon of length `n` and a set of possible ribbon lengths. We need to cut the ribbon into the maximum number of pieces that comply with the above-mentioned possible lengths. Write a method that will return the count of pieces.}

* Example 1

```Plaintext
n: 5
Ribbon Lengths: {2,3,5}
Output: 2
Explanation: Ribbon pieces will be {2,3}.
```

* Example 2

```Plaintext
n: 7
Ribbon Lengths: {2,3}
Output: 3
Explanation: Ribbon pieces will be {2,2,3}.
```

* Example 3

```Plaintext
n: 13
Ribbon Lengths: {3,5,7}
Output: 3
Explanation: Ribbon pieces will be {3,3,7}.
```

## Problem Statement

\ques{Given a number array to represent possible ribbon lengths and a total ribbon length `n`, we need to find the maximum number of pieces that the ribbon can be cut into.}

\hint{This problem follows the [Unbounded Knapsack](/unbounded-knapsack/) pattern and is quite similar to [Minimum Coin Change](/unbounded-knapsack/minimum-coin-change) (MCC). The only difference is that in MCC, we were asked to find the **minimum** number of coin changes, whereas, in this problem, we need to find the **maximum** number of pieces.}

## Basic Solution

A basic brute-force solution could be to try all combinations of the given lengths to select the maximum one that gives the total length of `n`. This is what our algorithm will look like:

```Plaintext
for each length 'l' 
    create a new set which includes one quantity of length 'l' if it does not exceed 'n', and 
        recursively call to process all lengths 
    create a new set without length 'l', and recursively call to process the remaining lengths
return the number of pieces from the above two sets with a higher number of pieces
```

### Code

Here is the code for the brute-force solution:

```python
import math


def count_ribbon_pieces(ribbonLengths, total):
    maxPieces = count_ribbon_pieces_recursive(ribbonLengths, total, 0)
    return -1 if maxPieces == -math.inf else maxPieces


def count_ribbon_pieces_recursive(ribbonLengths, total, currentIndex):
    # base check
    if total == 0:
        return 0

    n = len(ribbonLengths)
    if n == 0 or currentIndex >= n:
        return -math.inf

    # recursive call after selecting the ribbon length at the currentIndex
    # if the ribbon length at the currentIndex exceeds the total, we shouldn't process this
    c1 = -math.inf
    if ribbonLengths[currentIndex] <= total:
        result = count_ribbon_pieces_recursive(
            ribbonLengths, total - ribbonLengths[currentIndex], currentIndex)
        if result != -math.inf:
            c1 = result + 1

    # recursive call after excluding the ribbon length at the currentIndex
    c2 = count_ribbon_pieces_recursive(ribbonLengths, total, currentIndex + 1)
    return max(c1, c2)


def main():
    print(count_ribbon_pieces([2, 3, 5], 5))
    print(count_ribbon_pieces([2, 3], 7))
    print(count_ribbon_pieces([3, 5, 7], 13))
    print(count_ribbon_pieces([3, 5], 7))


main()
```

\comp{The above algorithm’s time complexity is exponential $O(2^{L+T})$, where $L$ represents total ribbon lengths, and $N$ is the total length that we want to cut. The space complexity will be $O(L+T)$.}

Since this problem is quite similar to [Minimum Coin Change](/unbounded-knapsack/minimum-coin-change), let’s jump on to the bottom-up dynamic programming solution.

## Bottom-up Dynamic Programming


Let’s try to populate our array `dp[ribbonLength][total+1]` for every possible ribbon length with a maximum number of pieces.

So for every possible length `len` (0 <= len <= total) and for every possible ribbon length index (0 <= index < ribbonLengths.length), we have two options:

* **Exclude the ribbon length**: In this case, we will take the maximum piece count from the previous set => `dp[index-1][len]`

* **Include the ribbon length** if its value is not more than `len`: In this case, we will take the maximum pieces needed to get the remaining total, plus include `1` for the current ribbon length => `1 + dp[index][len-ribbonLengths[index]]`

Finally, we will take the maximum of the above two values for our solution:

```Plaintext
dp[index][len] = max(dp[index-1][len], 1 + dp[index][len-ribbonLengths[index]])
```

### Code

Here is the code for our bottom-up dynamic programming approach:

```python
import math


def count_ribbon_pieces(ribbonLengths, total):
    n = len(ribbonLengths)
    dp = [[-math.inf for _ in range(total+1)] for _ in range(n)]

    # populate the total=0 columns, as we don't need any ribbon to make zero total
    for i in range(n):
        dp[i][0] = 0

    for i in range(n):
        for t in range(1, total+1):
            if i > 0:  # exclude the ribbon
                dp[i][t] = dp[i - 1][t]
            # include the ribbon and check if the remaining length can be cut into available lengths
            if t >= ribbonLengths[i] and dp[i][t - ribbonLengths[i]] != -math.inf:
                dp[i][t] = max(dp[i][t], dp[i][t - ribbonLengths[i]] + 1)

    # total combinations will be at the bottom-right corner, return '-1' if cutting is not possible
    return -1 if dp[n - 1][total] == -math.inf else dp[n - 1][total]


def main():
    print(count_ribbon_pieces([2, 3, 5], 5))
    print(count_ribbon_pieces([2, 3], 7))
    print(count_ribbon_pieces([3, 5, 7], 13))
    print(count_ribbon_pieces([3, 5], 7))


main()
```

\comp{The above solution has time and space complexity of $O(L*N)$, where $L$ represents total ribbon lengths and $N$ is the total length that we want to cut.}

## Next
@@flist
* \goto{/fibonacci-numbers/} Fibonacci Numbers
@@



