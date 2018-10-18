---
layout: post
title: Python Tricks
category: Python
tags: Python  
keywords: Python 
published: true
---

# Python Tricks

## Format number with leading `0`

```python
>>> print('data/osrmDist_{0:07d}_{1:07d}.csv'.format(0, 999))
data/osrmDist_0000000_0000999.csv
```

## pickle

```python
import pickle

with open('tmp.pickle', 'wb') as f:
    pickle.dump(df, f)

with open('tmp.pickle', 'rb') as f:
    df = pickle.load(f)
```

## Pause running by `time.sleep()`

```python
import time
time.sleep(0.005)  # sleep 5 milli-seconds
```

## Access URL and retrieve JSON

```python
# Python3.6
import urllib.request
import json

def retrieveJSON(url):
    result = None
    # with urllib.request.urlopen("http://maps.googleapis.com/maps/api/geocode/json?address=google") as url:
    with urllib.request.urlopen(url) as req:
        result = json.loads(req.read().decode())
    return result
```

## Access URL by Proxy

```python
proxy_support = urllib.request.ProxyHandler({
    'http': 'http://aaaaaaaa.com:8080' 
})
opener = urllib.request.build_opener(proxy_support)
urllib.request.install_opener(opener)
url = "http://router.project-osrm.org/route/v1/driving/-94.19466,36.35858;-112.40870,34.55132?overview=false"
with urllib.request.urlopen(url) as req:
    print(req.read().decode())
```

## Compare `nested loop` vs. `itertools.product`

```python
def f1():
    I = ["store-{}".format(i) for i in range(10000)]
    J = ["DC-{}".format(j) for j in range(1000)]
    A = []
    for i in I:
        for j in J:
            A.append("i = {}   j={}".format(i, j))
    print(len(A))
    
import itertools
def f2():
    I = ["store-{}".format(i) for i in range(10000)]
    J = ["DC-{}".format(j) for j in range(1000)]
    A = []
    for (i, j) in itertools.product(I, J):
        A.append("i = {}   j={}".format(i, j))
    print(len(A))

def f3():
    I = ["store-{}".format(i) for i in range(10000)]
    J = ["DC-{}".format(j) for j in range(1000)]
    A = ["i = {}   j={}".format(i, j) for i in I for j in J]
    print(len(A))
    
def f4():
    I = ["store-{}".format(i) for i in range(10000)]
    J = ["DC-{}".format(j) for j in range(1000)]
    A = ["i = {}   j={}".format(i, j) for i, j in itertools.product(I, J)]
    print(len(A))

%timeit  f1()  # 4.17 s ± 25.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
%timeit  f2()  # 4.3 s ± 37.1 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
%timeit  f3()  # 3.68 s ± 126 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
%timeit  f4()  # 4.08 s ± 279 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```
