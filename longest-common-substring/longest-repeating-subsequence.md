@def hascode = true

# Longest Repeating Subsequence

\blurb{We'll cover the following}

\lineskip

\toc

\lineskip

## Problem Statement

\ques{Given a sequence, find the length of its longest repeating subsequence (LRS). A repeating subsequence will be the one that appears at least twice in the original sequence and is not overlapping (i.e. none of the corresponding characters in the repeating subsequences have the same index).}

* Example 1

```Plaintext
Input: “t o m o r r o w”
Output: 2
Explanation: The longest repeating subsequence is “or” {tomorrow}.
```

* Example 2

```Plaintext
Input: “a a b d b c e c”
Output: 3
Explanation: The longest repeating subsequence is “a b c” {a a b d b c e c}.
```

* Example 3

```Plaintext
Input: “f m f f”
Output: 2
Explanation: The longest repeating subsequence is “f f” {f m f f, f m f f}. Please note the second last character is shared in LRS.
```
## Basic Solution

The problem is quite similar to the [Longest Common Subsequence](/longest-common-substring/longest-common-subsequence/) (LCS), with two differences:

* In LCS, we were trying to find the longest common subsequence between the two strings, whereas in LRS we are trying to find the two longest common subsequences within one string.
* In LRS, every corresponding character in the subsequences should not have the same index.

A basic brute-force solution could be to try all subsequences of the given sequence to find the longest repeating one, but the problem is how to ensure that the LRS’s characters do not have the same index. For this, we can start with two indexes in the given sequence, so at any step we have two choices:

* If the two indexes are not the same and the characters at both the indexes are same, we can recursively match for the remaining length (i.e. by incrementing both the indexes).
* If the characters at both the indexes don’t match, we start two new recursive calls by incrementing each index separately. The LRS would be the one with the highest length from the two recursive calls.

### Code

Here is the code:
```python
def find_LRS_length(str):
    return find_LRS_length_recursive(str, 0, 0)


def find_LRS_length_recursive(str,  i1,  i2):
    if i1 == len(str) or i2 == len(str):
        return 0

    if i1 != i2 and str[i1] == str[i2]:
        return 1 + find_LRS_length_recursive(str, i1 + 1, i2 + 1)

    c1 = find_LRS_length_recursive(str, i1, i2 + 1)
    c2 = find_LRS_length_recursive(str, i1 + 1, i2)

    return max(c1, c2)


def main():
    print(find_LRS_length("tomorrow"))
    print(find_LRS_length("aabdbcec"))
    print(find_LRS_length("fmff"))


main()
```

\comp{The time complexity of the above algorithm is exponential $O(2^n)$, where $n$ is the length of the input sequence. The space complexity is $O(n)$ which is used to store the recursion stack.}

## Top-down Dynamic Programming with Memoization

We can use an array to store the already solved subproblems.

The two changing values to our recursive function are the two indexes, i1 and i2. Therefore, we can store the results of all the subproblems in a two-dimensional array. (Another alternative could be to use a hash-table whose key would be a string (i1 + “|” + i2)).

### Code

Here is the code:

```python
def find_LRS_length(str):
    n = len(str)
    dp = [[-1 for _ in range(n)] for _ in range(n)]
    return find_LRS_length_recursive(dp, str, 0, 0)


def find_LRS_length_recursive(dp,  str, i1, i2):
    n = len(str)
    if i1 == n or i2 == n:
        return 0

    if dp[i1][i2] == -1:
        if i1 != i2 and str[i1] == str[i2]:
            dp[i1][i2] = 1 + find_LRS_length_recursive(dp, str, i1 + 1, i2 + 1)
        else:
            c1 = find_LRS_length_recursive(dp, str, i1, i2 + 1)
            c2 = find_LRS_length_recursive(dp, str, i1 + 1, i2)
            dp[i1][i2] = max(c1, c2)

    return dp[i1][i2]


def main():
    print(find_LRS_length("tomorrow"))
    print(find_LRS_length("aabdbcec"))
    print(find_LRS_length("fmff"))


main()
```

## Bottom-up Dynamic Programming

Since we want to match all the subsequences of the given string, we can use a two-dimensional array to store our results. As mentioned above, we will be tracking two indexes to overcome the overlapping problem. So for each of the two indexes, `i1` and `i2`, we will choose one of the following options:

* If `i1` and `i2` are different and the character `str[i1]` matches the character `str[i2]`, then the length of the LRS would be one plus the length of LRS up to `i1-1` and `i2-1` indexes.

* If the character at `str[i1]` does not match `str[i2]`, we will take the LRS by either skipping `i1`th or `i2`th character.

So our recursive formula would be:

```python
if i1 != i2 && str[i1] == str[i2] 
    dp[i1][i2] = 1 + dp[i1-1][i2-1]
else 
    dp[i1][i2] = max(dp[i1-1][i2], dp[i1][i2-1])
```

### Code

Here is the code for our bottom-up dynamic programming approach:

```python
def find_LRS_length(str):
    n = len(str)
    dp = [[0 for _ in range(n+1)] for _ in range(n+1)]
    maxLength = 0
    # dp[i1][i2] will be storing the LRS up to str[0..i1-1][0..i2-1]
    # this also means that subsequences of length zero(first row and column of
    # dp[][]), will always have LRS of size zero.
    for i1 in range(1, n+1):
        for i2 in range(1, n+1):
            if i1 != i2 and str[i1 - 1] == str[i2 - 1]:
                dp[i1][i2] = 1 + dp[i1 - 1][i2 - 1]
            else:
                dp[i1][i2] = max(dp[i1 - 1][i2], dp[i1][i2 - 1])

            maxLength = max(maxLength, dp[i1][i2])

    return maxLength


def main():
    print(find_LRS_length("tomorrow"))
    print(find_LRS_length("aabdbcec"))
    print(find_LRS_length("fmff"))


main()
```

\comp{The time and space complexity of the above algorithm is $O(n^2)$, where $n$ is the length of the input sequence.}

## Next
@@flist
* \goto{/longest-common-substring/subsequence-pattern-matching} Subsequence Pattern Matching
@@