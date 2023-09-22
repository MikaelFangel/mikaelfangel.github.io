---
title: Longest Palindromic Subsequence
description: Write-up on how to find the longeste palindrome that is a subsequence of a string
slug: longest-palindrome
date: 2023-09-20 00:00:00+0000
image: 
categories:
    - Algorithms
tags:
    - Algorithms
    - Dynamic Programming
    - Java
    - Longest Common Subsequence 
    - LCS
    - Solution
weight: 1
---

A palindrome is a non-empty string that exhibits symmetry when read forwards and backwards. Examples of palindromes include “anna,” “racecar,” and “abba.”

## Problem

Determine the longest palindromic subsequence of a given string. To find the palindrome you are allowed to delete characters in the string.

### Input

A string of length n

### Output

The length of the longest palindrome

## Solution Approach

To address this problem, we leverage the Longest Common Subsequence (LCS) algorithm, a dynamic programming technique. This approach entails breaking the problem into smaller, more manageable subproblems.

In our context, the core issue revolves around deciding whether a character should be included in the longest palindromic subsequence or not. The hallmark of palindromes is their symmetric nature, reading the same forwards and backwards. To harness this property, we apply the LCS algorithm to both the original string and its reversed counterpart.

This systematic application of LCS facilitates the efficient identification of the longest palindromic subsequence within the given string.

### Implementation

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class LongestPalindrome {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String input = br.readLine();
        int n = input.length();

        int[][] matrix = new int[n + 1][n + 1];
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if(input.charAt(n - j) == input.charAt(i - 1)) {
                    matrix[i][j] = matrix[i - 1][j - 1] + 1;
                } else {
                    matrix[i][j] = Math.max(matrix[i - 1][j], matrix[i][j - 1]);
                }
            }
        }

        System.out.println(matrix[n][n]);
    }
}
```
