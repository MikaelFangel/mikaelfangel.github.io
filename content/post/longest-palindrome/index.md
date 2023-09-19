---
title: Longest Palindrome
description: Write-up on how to find the longeste palindrome that is a subsequence of a string
slug: longest-palindrome
date: 2023-09-19 00:00:00+0000
image: 
categories:
    - Algorithms
tags:
    - Algorithms
    - Dynamic Programming
    - Java
    - Longest Common Subsequence 
    - LCS
weight: 1
---

# Longest Palindrome
A palindrome is a non empty string that reads the same forwards and backwards. Examples of palindromes is thus *anna*, *racecar*, *abba* and so forth.
d
## Problem
Find the longest palindrome which is a subsequence of a given string. To find the palindrome you are allowed to delete characters in the string.

### Input
A string of length n

### Output
The length of the longest palindrome

## Solution
To solve the problem we can use the Longest Common Subsequence (LCS) algorithm to find the answer. The algorithm is a dynamic programming algorithm, meaning that to solve the problem we must break it up into smaller problems.  

The problem in our case is either a character is a part of the longest palindrome or it is not. To find it using LCS we exploit the property of the palindrome that it is read the same backwards and forwards. This means that we can use LCS on the original string and the reversed.

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