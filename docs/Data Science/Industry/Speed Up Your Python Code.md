
## Learn to use itertools library
Python’s [Itertools](https://docs.python.org/3/library/itertools.html) is a module that provides various functions that work on iterators to produce complex iterators. This module works as a fast, memory-efficient tool that is used either by themselves or in combination to form iterator algebra.

## Use proper data structure
Use of proper data structure has a significant effect on runtime. Python has list, tuple, set and dictionary as the built-in data structures.

However, most of the people use the list in all cases. But it is not a right choice. Use proper data structures depending on your task. 

Especially use a tuple instead of a list. Because iterating over tuple is easier than iterating over a list.

## Use generators
If you have a large amount of data in your list and you need to use one data at a time and for once then use generators. It will save you time. 

## Use list comprehensions

```python
cube_numbers = []
for n in range(0,10):
	if n % 2 == 1:
		cube_numbers.append(n**3)
```

Don't use such loops, Instead use list comprehension approach as shown below

```python
cube_numbers = [n**3 for n in range(1,10) if n%2 == 1]
```

## Use sets and unions
```python
a = [1,2,3,4,5]
b = [2,3,4,5,6]

overlaps = []
for x in a:
  for y in b:
    if x==y:
      overlaps.append(x)

print(overlaps)
```
This will print the list [2, 3, 4, 5]

Try not using such for nested loops Rather use sets and unions when ever you can

```python
a = [1,2,3,4,5]
b = [2,3,4,5,6]

overlaps = set(a) & set(b)

print(overlaps)
```
This will print the dictionary {2, 3, 4, 5}

## Use ``join()`` to concatenate strings

```
new = "This" + "is" + "going" + "to" + "require" + "a" + "new" + "string" + "for" + "every" + "word"
```

Don't use "+" to join strings rather use the str.join method

```
new = " ".join(["This", "will", "only", "create", "one", "string", "and", "we", "can", "add", "spaces."])
```

## Use multiple assignments 
Do not assaign variables like this:

```
a = 2
b = 3
c = 5
d = 7
```

Instead, assign variables like this:
```
 a, b, c, d = 2, 3, 5, 7
```
## Do not use dot operation
Because when you call a function using . (dot) it first calls __getattribute()__ or __getattr()__ which then use dictionary operation which costs time. So, try using from module import function.

```python
import math
val = math.sqrt(60)
```
Rather import at like this:
```python
from math import sqrt
val = sqrt(60)
```
