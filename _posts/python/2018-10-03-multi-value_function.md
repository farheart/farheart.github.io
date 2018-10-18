---
layout: post
title: About `func(*args, **kwargs)`
category: Python
tags: Python  
keywords: Python 
published: true
---

# About `func(*args, **kwargs)`

> Ref: <https://www.jianshu.com/p/ebc19de7a204?utm_campaign=maleskine&utm_content=note&utm_medium=pc_all_hots&utm_source=recommendation>

Use to handle uncertain number of arguments to a function

Two types of multi-value arguments
* `*arg` : one `*` before argument name : **tuple** parameter
* `**kwargs` : two `*` before argument name : **dict** parameter

Example

```python
def demo(num, *args, **kwargs):
    print(num)
    print(args)
    print(kwargs)
    print('-'*10)

demo(1, 2, 3, 4, 5, name='ab', age= 18, gender=True)
demo(1, (2, 3, 4, 5), {'name':'ab', 'age':18, 'gender':True})
demo(1, (2, 3, 4, 5), name='ab', age= 18, gender=True)

a = (2, 3, 4, 5)
b = {'name':'ab', 'age':18, 'gender':True}
demo(1, a, b)
demo(1, *a, **b)  # 拆包
```

output

```bash
1
(2, 3, 4, 5)
{'name':'ab', 'age':18, 'gender':True}
----------
1
((2, 3, 4, 5), {'name':'ab', 'age':18, 'gender':True})
{}
----------
1
((2, 3, 4, 5),)
{'name':'ab', 'age':18, 'gender':True}
----------
1
((2, 3, 4, 5), {'name':'ab', 'age':18, 'gender':True})
{}
----------
1
(2, 3, 4, 5)
{'name':'ab', 'age':18, 'gender':True}
```