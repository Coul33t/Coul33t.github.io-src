Title: Python tricks
Date: 2019-04-11 14:00
Modified: 2019-04-11 14:00
Tags: Python Tricks
Category: Python
Slug: Python-tricks
Authors: Quentin Couland
Summary: A few tricks in Python

## 1) Iterate over a list pairwise
---
Let *l* be a list containing *n* elements. We want to run through it pairwise : **0/1, 1/2, ..., (n-1)/n**

e.g. `l = [0,1,2,3,4,5,6,7,8,9]` -> `0/1, 1/2, 2/3, ..., 8/9`

```
for a, b in zip(l[:-1], l[1:]):
    print(f'a = {a}, b = {b}')
```
or
```
from itertools import tee

def pairwise(iterable):
    a, b = tee(iterable)
    next(b, None)
    return zip(a, b)

for a, b in pairwise(l):
    print(f'a = {a}, b = {b}')
```
The latter is a bit faster than the former.

### Time comparison:
`l = [x for x in range(1000000)]`

| Algorithm | Naive   | Naive (pre-computed limit) | zip()   | pairwise() |
|-----------|---------|----------------------------|---------|------------|
| Time      | 0.46799 |           0.33899          | 0.07999 |   0.07700  |