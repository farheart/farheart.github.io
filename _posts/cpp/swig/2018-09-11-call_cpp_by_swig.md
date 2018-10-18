---
layout: post
title: Python call C++ function by SWIG
category: Cpp
tags: Python  Cpp
keywords: Python  SWIG  C++
published: true
---

# Python call C++ function by SWIG

This note shows how to use `SWIG` to enable **python** invoking `C++` codes, which might be wanted if high-efficiency is needed, such as create `cplex` model

## What is `SWIG`
`SWIG` is an interface compiler that connects programs written in `C` and `C++` with scripting languages such as `Perl`, `Python`, `Ruby`, and `Tcl`. It works by taking the declarations found in `C/C++` header files and using them to generate the wrapper code that scripting languages need to access the underlying `C/C++` code.

## Install

> Ref: <https://blog.csdn.net/LIANGJIANGLI/article/details/72832154> (Mac Swig 3.0.12 安装)

Steps

* Download, Extract 
* `./configure --prefix=/Users/y0w02p1/wy/programs/swig // 指定安装目录`
  * 如果想让默认安装的话，可以直接执行 `./configure`
* `make  // compiling`
* `make install  // do install`
* Configurate path: `nano ~/.bash_profile`
    ```bash
    export SWIG_HOME=/Users/y0w02p1/wy/programs/swig
    export PATH=${PATH}:${SWIG_HOME}/bin
    ```
* Test: `$ swig -version` 显示版本号说明安装成功！

## Example (Python to C)

### Codes

```c
/* File: example.i */
%module example
%{
    /* Put header files here or function declarations like below */
    extern double My_variable;
    extern int fact(int n);
    extern int my_mod(int x, int y);
    extern char *get_time();
    extern char *getTime();
%}

extern double My_variable;
extern int fact(int n);
extern int my_mod(int x, int y);
extern char *get_time();
extern char *getTime();
```

```c
/* File : example.c */
#include <time.h>
double My_variable = 3.0;

int fact(int n) {
    if (n <= 1) return 1;
    else return n*fact(n-1);
}

int my_mod(int x, int y) {
    return (x%y);
}

char *get_time()
{
    time_t ltime;
    time(&ltime);
    return ctime(&ltime);
}

char *getTime() {
    time_t tVal;
    time_t* p = &tVal;
    time(p);
    return ctime(p);
}
```

#### Compile

* Create example_wrap.c and example.py using swig

```bash
swig -python example.i
```

* Compile .c to .o file

```bash
gcc  -c example.c  example_wrap.c -I/usr/local/anaconda3/envs/django/include/python3.6m
```

* Link .o to .so library

```bash
gcc  -bundle -undefined dynamic_lookup -L/usr/local/anaconda3/envs/django/lib example.o example_wrap.o  -o _example.so
```

#### Python test

*Note*: Since we compile it using **conda - django - Python3.6**, before test, need to activate the conda env first

```bash
conda activate django
python
```

```python
# --------------------------------------
# Test example module
# --------------------------------------
import example
print(example.fact(5))      # 120
print(example.my_mod(5,3))  # 2
print(example.get_time())   # current time
print(example.getTime())    # current time
```

### To address the issue in compiling

> Ref: <https://stackoverflow.com/questions/25882150/python-h-not-found-using-swig-and-anaconda-python>

You can use the command `python-config --cflags` to get the full set of recommended compilation flags:

```bash
python-config --cflags
```

> -I/Users/myuser/anaconda/include/python2.7  -I/Users/myuser/anaconda/include/python2.7 -fno-strict-aliasing -I/Users/myuser/anaconda/include -arch x86_64 -DNDEBUG -g -fwrapv -O3 -Wall -Wstrict-prototypes

Let `distutils` figure out all the compiling and linking options for you.

```python
# File: setup.py
from distutils.core import setup, Extension
example_module = Extension('_example', sources=['example.c', 'example.i'])
setup(name='example', ext_modules=[example_module], py_modules=["example"])
```

Then you can run:

```bash
python setup.py build_ext --inplace
```

## Reference

* SWIG Official 

<http://www.swig.org/exec.html> (What is SWIG)
<http://www.swig.org/tutorial.html> (Tutorial)

* 利用 SWIG 对 C++ 库进行 Python 包装

<https://segmentfault.com/a/1190000013219667>

* 如何实现 C/C++ 与 Python 的通信？

<https://www.zhihu.com/question/23003213/answer/56121859>
> 本文分4个部分C/C++ 
> * 调用 Python （基础篇）— 仅讨论Python官方提供的实现方式Python 
> * 调用 C/C++ （基础篇）— 仅讨论Python官方提供的实现方式C/C++ 
> * 调用 Python （高级篇）— 使用 CythonPython 
> * 调用 C/C++  （高级篇）— 使用 SWIG
