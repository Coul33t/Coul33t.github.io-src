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


## 2) Quickly rename files in a folder

```
import os
[os.rename(f, f.replace('OLD', 'NEW')) for f in os.listdir('PATH_TO_FOLDER')]
```

You can add a condition, like a string found in the file name:

```
import os
[os.rename(f, f.replace('OLD', 'NEW')) for f in os.listdir('PATH_TO_FOLDER') if 'STRING_TO_FIND' in f]
```

## 3) Quickly rename files in a folder and all subfolders
```
import os
for root, dirs, files in os.walk(r'PATH_TO_FOLDER'):
    for i in files:
        if 'STRING_TO_FIND' in i:
            print(os.path.join(root, i))
            os.rename(os.path.join(root, i), os.path.join(root, i.replace('SUBSTRING_TO_REPLACE', 'SUBSTRING_REPLACEMENT')))
```