---
title: "Programming Tips"
date: 2018-11-12T02:39:54+00:00
slug: "programming-tips"
draft: false
description: "My list of Python tips for programming in Python and using it with Jupyter "
cover:
  image: "/images/2018/11/python.JPG"
  relative: false
  hiddenInList: false
tags: ["Python", "programming"]
---

This is a list of python tips and techniques that I have run across when using this language. It also serves as a quick reminder/refresher page for me. It is a mix of general python and python in Jupyter.

# Lists

### Count the total number of unique items in list

```
A = {}
for i in my_list:
	A[i] = i
len(A)
```

### Count each unique item in list

```
A = {}
for i in mylist:
    if i not in A:
        A[i]=1
    else:
        A[i]=A[i]+1
print(A)
```

# Jupyter

### Plotting in Jupyter

In order to plot inline, use the following

```
%matplotlib inline
```

### Scatter plots

Scatter plots require a third variable to indicate the size of the dot. Without this variable, the scatter command fails.

```
import matplotlib.pyplot as plt
plt.scatter(x,y,0.1)
```

# Counting

### Counting in binary

Starting from the first digit, everything is halved each time you put 1 in the next place, so shift the pointer by halving the previous value.

```
1000000       1100000
---|---       -|-----
```
