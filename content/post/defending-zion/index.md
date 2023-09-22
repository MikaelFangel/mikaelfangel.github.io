---
title: Defending Zion
description: How to solve and approach the problem of defending zion.
slug: defending-zion
date: 2023-09-22 00:00:00+0000
image: cover.jpg
categories:
    - Algorithms
tags:
    - Algorithms
    - Dynamic Programming
    - Recurrence Relation
    - Solution
weight: 1
---

# Defending Zion

## Problem

You are given a time period of n seconds, during which waves of robots arrive. Each wave consists of Xi robots, and you have an EMP that can be used to defeat them. The EMP has a charge, denoted by the function f(j), which recharges after use. The goal is to find the optimal times to activate the EMP to maximize the total number of robots defeated over the entire period n.

### Input

- The first line contains the value of n, representing the duration of the time period.
- The second line contains n integers, Xi, representing the number of robots in each wave.
- The third line contains n values representing the output of the function f(j) at each second.

### Output

A single line containing the maximum number of robots defeated.

## Solution Approach

To solve this problem, we can make some key observations:

1. We cannot defeat more robots than there are in a given wave or more robots than the EMP charge allows.
2. The number of robots defeated in a wave is given by min(Xi, f(j)), where Xi is the number of robots in that wave, and f(j) is the EMP charge.
3. The number of robots that can be defeated in the next iteration depends on min(Xi+1, f(1)), as the EMP charge resets at each iteration.

We can express the problem using the following recurrence relation:

```
OPT(i, j) = max(min(Xi, f(j)) + OPT(i + 1, 1), OPT(i + 1, j + 1))
```

Here, OPT(i, j) represents the maximum number of robots defeated starting from the i'th second with EMP charge j. This recursive relation helps us find the optimal strategy for using the EMP throughout the time period.

> Photo by [ANIRUDH](https://unsplash.com/@lanirudhreddy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplah](https://unsplash.com/photos/UVa6OF2XXIc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)