---
layout: post
title: Python Decorator
category: Python
tags: Python
keywords: Python  Decorator
published: true
---
# Python Decorator

> Ref: Python 装饰器为什么难理解？<https://zhuanlan.zhihu.com/p/27449649>

## Background
- Everything is an object in Python, so does a function
- A function can be used as an argument passing into another function
- A function can be used as a return value as well

## A simple example
```python
def wrap(aFunc):
    def wrapped():
        print("Starting ...")
        aFunc()  # closure
        print("End")
    return wrapped

def foo():
    print("foo")

foo = wrap(foo)  # wrap / decorate 
foo()


# Using decorator
@wrap
def foo():
    print("foo")

foo()
```

This is decorator

[DONE]


