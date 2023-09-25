---
title: Solutions to Functional Programming Using F# Chap. 4
description: My solutions to the 4th chapter of the book "Functional Programming Using F# by Michael R. Hansen and Hans Rischel"
slug: function-programming-chap4
date: 2023-09-23 00:00:00+0000
image: 
categories:
    - Functional Programming
tags:
    - Functional Programming
    - F#
    - Solution
    - Functional Programming Using F#
weight: 1
---

Here I'll post the code for my solutions to the 4th chapter of the book. The solutions for the book is made by me as a part of the course "02157 Functional Programming" on DTU. I'll update the post with more solutions when and if I solve more of the exercises.

## 4.3

In this function we should generate the first n even numbers. I use tail recursion for speed and to avoid stackoverflow. Because we should generate the first n even numbers we always start the iterator from 0 and then we can just increment the iterator i with 2 each time. By incrementing by 2 each time we avoid a check for if the numbers are even or not.

```fsharp
let evenN n =
    let rec evenNInner acc = function
    | i when i <= n -> evenNInner (i::acc) (i + 2) 
    | _ -> List.rev acc

    evenNInner [] 0 
```

## 4.7

We should find the number of times x occurs in a given list. To do this we split the list using the cons operator and then compare the head with the given x. Each time there is a match we add one to the result and continue the recursion. If there is no match we just recurse without adding anything. This effectively gives us the number of time x occurs in the list.

```fsharp
let rec multiplicity x = function
    | [] -> 0
    | head :: tail when x = head -> 1 + multiplicity x  tail
    | _ :: tail -> multiplicity x tail 
```

## 4.9

We should write a zip function and because a zip list is just a list of tuples containing the contents of both list we contruct this using a simple match pattern. We also throws an error if the list are of uneven lengths as it is thus not possible to zip the two lists.

```fsharp
let rec zip = function
    | [], [] -> []
    | x::xs, y::ys -> (x, y)::zip(xs, ys)
    | _ -> failwith "zip: The lists are of uneven lengths"
```

## 4.11

```fsharp
let rec count (l, x) =
    match l with
    | [] -> 0
    | l::ls when l = x -> 1 + count(ls, x)
    | _::ls -> count(ls, x)

let rec insert (l, x) =
    match l with 
    | [] -> []
    | l::ls when x <= l -> x::ls
    | l::ls -> l::insert(ls, x)

let rec intersect = function
    | [], _ | _, [] -> []
    | x::xs, y::ys when x = y -> x :: intersect (xs, ys)
    | x::xs, y::ys when x < y -> intersect (xs, y::ys)
    | x::xs, _::ys -> intersect (x::xs, ys)

let rec plus = function
    | [], ys -> ys
    | xs, [] -> xs
    | x::xs, y::ys when x <= y -> x :: plus (xs, y::ys)
    | xs, y::ys -> y :: plus (xs, ys)

let rec minus = function
    | [], _ -> []
    | xs, [] -> xs 
    | x::xs, y::ys when x = y -> minus (xs, ys)
    | x::xs, y::ys when x < y -> x :: minus(xs, y::ys)
    | x::xs, _::ys -> minus(x::xs, ys)
```

## 4.12

Here we should sum up all elements where the predicate p is true. This we do by using a match pattern with a when statement. If the predicate is true we just recurse until the list is empty and then returning 0.

```fsharp
let rec sum ((p:int -> bool), (xs)) =
    match xs with
    | [] -> 0
    | x::xs when p(x) -> x + sum(p, xs)
    | _::xs -> sum(p, xs)
```

 > Book by [Michael R. Hansen and Hans Richel](https://www.cambridge.org/us/universitypress/subjects/computer-science/programming-languages-and-applied-logic/functional-programming-using-f?format=HB&isbn=9781107019027)