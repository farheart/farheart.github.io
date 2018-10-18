---
layout: post
title: Passing data between Python and C++ by SWIG
category: C++
tags: SWIG  C++
keywords: Python  SWIG  C++
published: true
---

# Passing data between Python and C++ by SWIG

## Example - Pass `dict` from Python to C++

This example uses only one `.i` file, and the function is implemented inside the `.i` file.

### Codes

```c
// File: dictTest.i
%module dictTest

%include "std_string.i"
%include "std_map.i"

namespace std {
    %template(Str2StrMap) map<string, string>;
}

void foo(const std::map<std::string, std::string> &val);

%{
#include <iostream>
#include <string>
#include <map>

using namespace std;

void
foo(const map<string, string> &val)
{
    map<string, string>::const_iterator i = val.begin();
    map<string, string>::const_iterator end = val.end();
    while (i != end) {
        cout << i->first << " : " << i->second << endl;
        ++i;
    }
}
%}
```

### Compile

The following cmd creates dictTest.py, dictTest_wrap.cxx

```bash
swig  -python  -c++  dictTest.i
```

Compile and link to _dictTest.so
```bash
g++  -fPIC  -shared  -undefined dynamic_lookup -I/usr/local/anaconda3/envs/django/include/python3.6m -L/usr/local/anaconda3/envs/django/lib  -o _dictTest.so  dictTest_wrap.cxx
```

### Python test

```python
import dictTest

m = {'a' : 'b', 'c' : 'd'}
dictTest.foo(m)

# do dict to map converting first, not necessary
x = dictTest.Str2StrMap(m)
dictTest.foo(x)
```

### Reference
<https://stackoverflow.com/questions/9041192/how-does-swig-wrap-a-mapstring-string-in-python>

## Example - Pass `{(str, str): number}` from Python to C++

### Codes

```c
// File: dictTest2.i
%module dictTest2

%include "std_string.i"
%include "std_map.i"

namespace std {
    %template(Str2FloatMap) map<string, double>;
	
    // Tuple / pair as key of dict / map
    %template(StrPair2FloatMap) map<pair<string, string>, double>;
    
    // %template(IntPair2IntMap) map<pair<int, int>, int>;
}

void tryStr2Float(const std::map<std::string, double> &val);
void tryStrPair2Float(const std::map<std::pair<std::string, std::string>, double> &val);
// void tryIntPair2Int(const std::map<std::pair<int, int>, int> &val);

%{
#include <iostream>
#include <string>
#include <map>

using namespace std;

void
tryStr2Float(const map<string, double> &val) {
    map<string, double>::const_iterator i = val.begin();
    map<string, double>::const_iterator end = val.end();
    while (i != end) {
        cout << i->first << " : " << i->second << endl;
        i++;
    }
}

void
tryStrPair2Float(const map<pair<string, string>, double> &val) {
    map<pair<string, string>, double>::const_iterator i = val.begin();
    map<pair<string, string>, double>::const_iterator end = val.end();
    while (i != end) {
        cout << "(" << (i->first).first << "," << (i->first).second <<  ") : " << i->second << endl;
        i++;
    }
}
%}
```

### Compile

```bash
swig  -python  -c++  dictTest2.i
```

```bash
$ g++  -fPIC  -shared  -undefined dynamic_lookup  -I/usr/local/anaconda3/envs/django/include/python3.6m  -L/usr/local/anaconda3/envs/django/lib  -o _dictTest2.so  dictTest2_wrap.cxx
```

### Test

```python
import dictTest2

m1 = {k:k * 0.5 for k in range(5)}
dictTest2.tryStr2Float(m1)

m2 = {('x{}'.format(i), 'y{}'.format(i)): i for i in range(5)}
dictTest2.tryStrPair2Float(m2)
```

## Example - `dict` in, `dict` out

> Ref: <https://stackoverflow.com/questions/38556223/returning-arguments-in-swig-python>

### Codes

#### Header file

```h
// File: mapIO.h
#pragma once

#include <iostream>
#include <string>
#include <unordered_map>

void printThenAdd(std::unordered_map<std::string, std::string>* inout) {
    for( const auto& n : (*inout) ) {
        std::cout << "Key:[" << n.first << "] Value:[" << n.second << "]\n";
    }
    (*inout)["BLACK"] = "#000000";
};
```

#### Interface file

```c
// File : mapIO.i
%module mapIO

%{
  #include "mapIO.h"
%}

%include "typemaps.i"

%typemap(in) std::unordered_map<std::string, std::string>* (std::unordered_map<std::string, std::string> temp) {
    PyObject *key, *value;
    Py_ssize_t pos = 0;

    $1 = &temp;
    temp = std::unordered_map<std::string, std::string>();

    while (PyDict_Next($input, &pos, &key, &value)) {
        (*$1)[PyUnicode_AsUTF8(key)] = std::string(PyUnicode_AsUTF8(value));
    }
}

%typemap(argout) std::unordered_map<std::string, std::string>* {
    $result = PyDict_New();
    for(const auto& n : *$1) {
        PyDict_SetItemString(
            $result, 
            n.first.c_str(),
            PyString_FromString(n.second.c_str())
        );
    }
}

%include "mapIO.h"
```

### Compile

Create `mapIO_wrap.cxx`
```bash
swig  -python  -c++  mapIO.i
```

Compile and link to `_mapIO.so`
```bash
g++  -fPIC  -shared  -undefined dynamic_lookup -I/usr/local/anaconda3/envs/django/include/python3.6m -L/usr/local/anaconda3/envs/django/lib  -o _mapIO.so  mapIO_wrap.cxx
```

#### Test

```python
import mapIO
m = {'WHITE' : '#FFFFFF'}
a = mapIO.printThenAdd(m)
```

Output is an updated dictionary

```bash
In[2]: a
Out[3] : {'BLACK': '#000000', 'WHITE': '#FFFFFF'}
```

## Example - `list` in, `tuple` out

### Codes

```c
// File: arrayTest.i
%module arrayTest

%include "std_vector.i"
%include "std_string.i"

// Tell SWIG about the templates you will use.
%template() std::vector<double>;  // no need to give name
%template() std::vector<int>;

// Declaration
template<typename T> std::vector<T> revert(const std::vector<T> &val);

// Generate wrappers from function template
%template(revertDouble) revert<double>;
%template(revertInt)    revert<int>;

%{
    #include <iostream>
    #include <string>

    using namespace std;

    template<typename T> vector<T> revert(const vector<T> &val) {
        vector<T> result;
        for (int i=val.size()-1; i>=0; i--) {
            result.push_back(val[i]);
            // cout << i << " : " << val[i] << endl;
        }
        return result;
    }
%}
```

### Compile

Create `arrayTest_wrap.cxx`
```bash
swig  -python  -c++  arrayTest.i
```

Compile and link to `_arrayTest.so`
```bash
g++  -fPIC  -shared  -undefined dynamic_lookup -I/usr/local/anaconda3/envs/django/include/python3.6m -L/usr/local/anaconda3/envs/django/lib  -o _arrayTest.so  arrayTest_wrap.cxx
```

### Python test

```python
import arrayTest

a = range(10)
b = arrayTest.revert(a)
print(b)
type(b)
```

### Reference

* swig, from list to non-standard vector
  <https://stackoverflow.com/questions/26040514/swig-from-list-to-non-standard-vector>

* Swig doc
  <http://www.swig.org/Doc1.3/SWIGPlus.html#SWIGPlus_nn30>
