---
title: Solutions to Functional Programming Using F# Chap. 1
description: My solutions to the 1st chapter of the book "Functional Programming Using F# by Michael R. Hansen and Hans Rischel"
slug: function-programming-chap1
date: 2024-05-04 00:00:00+0000
image: cover.jpg
categories:
    - Functional Programming
tags:
    - Functional Programming
    - F#
    - Solution
    - Functional Programming Using F#
weight: 1
---

Here I'll post the code for my solutions to the 1st chapter of the book.

## 1.1


```fsharp
let g n = n + 4
```

## 1.2

```fsharp
let h (x, y) = System.Math.Sqrt(x * x + y * y)
```

## 1.3

The function expression for `g` is of type `int -> int` and the function expression for `h` is `(float * float) -> float`


## 1.4
The recusion formula for the following problem is: 

```
0 = 0
n = n + (n - 1) for n > 0
```

The code for the assignment:

```fsharp
let rec f =
    function
    | 0 -> 0
    | n -> n + f (n - 1)
```

The evaluation for f 3 is as follows:

```
~> f 3
~> 3 + f (n - 1)
~> 3 + f 2
~> 3 + (2 + f(2 - 1)) 
~> 3 + (2 + f 1)
~> 3 + (2 + (1 + f(1 - 1))
~> 3 + (2 + (1 + f 0))
~> 3 + (2 + (1 + 0))
~> 3 + (2 + 1)
~> 3 + 3
~> 6
```

## 1.5

```fsharp
let rec fib = function
  | 0 -> 0
  | 1 -> 1
  | n -> fib (n - 1) + fib (n - 2)
```

The evaluation for fib 4 is as follows:

```
~> fib 4 
~> fib (4 - 1) + fib (4 - 2)
~> fib 3 + fib 2
~> (fib (3 - 1) + fib (3 - 2)) + (fib (2 - 1) + fib (2 - 2))
~> (fib 2 + fib 1) + (fib 1 + fib 0)
~> ((fib (2 - 1) + fib(2 - 2) + fib 1) + (1 + 0))
~> ((fib 1 + fib 0) + 1) + (1 + 0))
~> ((1 + 0) + 1) + (1 + 0))
~> (1 + 1) + (1 + 0))
~> 2 + 1
~> 3
```

## 1.6

The recursion formula for the following problem is:

```
(m, 0) = m
(m, n) = m + (m, n - 1)
```

The code for the solution: 

```fsharp 
let rec sum = function
  | (m, 0) -> m
  | (m, n) -> m + sum (m, n - 1)
```

## 1.7

The type for the expressions is:
(System.Math.PI, fact -1) = (float * int)
fact(fact 4) = int
power(System.Math.PI, fact 2) = float
(power, fact) = (float -> int -> float) * (int -> int))

 > Book by [Michael R. Hansen and Hans Richel](https://www.cambridge.org/us/universitypress/subjects/computer-science/programming-languages-and-applied-logic/functional-programming-using-f?format=HB&isbn=9781107019027)

 > Photo by [Ilija Boshkov](https://unsplash.com/@boshkov?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/photos/0nI1DczRQAM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)
  
