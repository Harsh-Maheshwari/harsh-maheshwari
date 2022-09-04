---
title: Data Structures & Algorithms
date: 2022-04-23 18:17:36
description:
tags: 
 - data_structures
 - python
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---

## Space Time Complexity

Aim of space time complexity theory is  to measure or quantify how much time and space a program or an algorithm takes in execution.

- Given a list has `n` elements, 
- Question : How many comparisons are needed in the worst case to find if a number `x` is in a list 
- Solution we would apply to solve this Query: Go one by one and check each element in the list and compare 
- In the worst case we would not have that element in the list and we would transverse the complete list
- Hence we would have to perform  `n`  comparisons.

``` python
n = 4
l = [1,2,3,4]
x = 5
for i in l:
	if i == x:
		print('the element exists')
```

Time Complexity for above problem is directly proportional to the length of the list 


## Further Reading : 

1. Data Structures and Algorithms
- https://jovian.ai/learn/data-structures-and-algorithms-in-python
- https://www.youtube.com/watch?v=pkYVOmU3MgA
- https://distill.pub/
- https://www.geeksforgeeks.org/data-structures/?ref=shm
- [Space & Time Complexity](https://www.geeksforgeeks.org/time-complexity-and-space-complexity/)

 