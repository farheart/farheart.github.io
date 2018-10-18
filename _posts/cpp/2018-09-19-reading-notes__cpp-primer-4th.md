---
layout: post
title: Reading Notes -- C++ Prime (4th)
category: C++
tags: C++  Reading_Note
keywords: C++
published: true
---

# Reading Notes -- C++ Prime (4th)

## Chapter-1 : Quick Tutorial (Pdf. 15 / 769)

### Pdf. 21

In/Out operator >> / << : return left operand

### Pdf.30

While (std::cin >> value) : test the status of istream std::cin

### Pdf. 32

Standard library’s head file use <XXX>, e.g., <iosteam>
Non-standard library’s head file use “YYY.h”, e.g, “wy_utit.h”

## Chapter-2 : Variable and Basic Type (Pdf. 43 / 769)

### Pdf. 53

lvalue can appear at either left side or right side of assignment statement
rvalue: only can appear at the right side of assignment statement

### Pdf. 56

Copy initialization : using =
Direct initialization: using ()

### Pdf. 57

`#include <string> // Before using string`
`std::string str_all_9 (5, ‘9’); // str_all_9 = “99999”`

### Pdf. 59, 60

Definition : only one place; is declaration as well

Declaration : use “extern” keyword to declare but not define; can happen multiple times

If a variable is used in multiple files, then one file has its definition, the others have its declaration.

### Pdf. 63

`const  int a = 123;` Here, `a` is still a lvalue, but 
* it can’t be modified anymore, and 
* it must be initialized immediately at definition.
* It only exists in its definition file scope

p.s. by default, const object is the local variable of its definition file (Pdf.73). If want external acess, then
`extern const int a = 123;`
Now `a` can be access from other files

### Pdf.65

const reference : point to const object
```c
const int a =123;
const int &r = a;  // OK
int &r1 = a;  // Wrong — non-const reference to a const object
```

### Pdf. 70

Define a **class** can use either `struct` or `class`, only the initial default access level will be impacted

* `struct`: default to public
* `class`: default to private

### Pdf. 73, 74

Header file is for declaration, not for definition.

* Because header file will be included in multiple files
* Containing definition will cause a link error

If a const variable is not initialized by a const expression, then it should not be defined in header file, instead it should be defined and initialized in a source file (.cpp). The header file should only contains its declaration with “extern	“

### Pdf. 75, 76

```cpp
#define
#ifndef
#endif

#include <header.h>  // header.h is a standard header and will be searched in system path

#include “header.h” // header.h is user-defined and will be searched starting from current path where the source file is at.
```

## Chapter-3 : Standard Library (Pdf. 81 / 769)

### Pdf. 82, 83

`::` scope operator — right operand can be found in the **scope** of left operand

e.g., `std::cin` : name `cin` is defined in the namespace `std`.

In header files, should **always** use the fully-qualified library name

### Pdf. 87

`size()` return `string::size_type`

### Pdf. 90

String’s `[]` operator returns lvalue — it can be assigned. e.g., 

```c
string s1 = “abc”; s1[1] = ‘2'; // s1 = string(“a2c”)
```

### Pdf. 93, 94

`vector<T> v4(n)`  : create n elements using default initializationd

Better to dynamically add element to vector (let it auto-increase), don’t pre-allocate spaces

### Pdf. 96

Why use `!=` as judge condition in for loop, should have an answer after generic programming (2nd section)

### Pdf. 101

`const` with **iterator**

```cpp
const vector<int> v(10, 1);  // 10 elements with value = 1

const vector<int>::iterator  ci = v.begin();  // ERROR: ci could change the element it refers to and v is const

vector<int>::const_iterator it = v.begin(); // OK: const_iterator can be used with const vector
*it = 2;  // ERROR: could not change element’s value — *it is const
++it;  // OK: can change refers to since the iterator itself is not const
```

## Chapter-4 : Array and Pointer (Pdf. 109 / 769)

### Pdf. 116

Two declaration style for pointer

* `string* ps1;  // one line one variable, * close to <type>
   string* ps2;`
* `string *ps1, *ps2;  // one line multiple variables, * close to var name`

### Pdf. 121

Arithmatic operations between pointers

```cpp
int[] ia = {1,2,3,4,5}
int *p1 = ia;
int *p2 = p1 + 4;
std::ptrdiff_t  n = p2 - p1;  // subtraction between pointers
// ptrdiff_t : defined in <cstdder> header file
```

### Pdf.124, 125, 126

```cpp
const double *cp;  
// cp points to a const object whose type is double, but cp itself is not const
// so we don’t have to initialize it when defining and we can change its value by re-assigning its value to point to another const var.
// cp thinks itself "pointing to a const object", but may not true.

double  d = 1.0
double *const pr = &d;  // pr is a constant pointer
// since pr itself is constant now, we can’t change its value anymore
// pr must be initialized at definition

const double PI =3.14;
const double *const ccp = &PI;
// neither *ccp nor ccp can be changed now
```

```cpp
typedef string *pt;  // pt is a type of "pointer to string"
const pt v;  // const applies to <type>, not <var>

// equivalent to 
string *const v;

// not equivalent to
const string *v;

// all 3 decreations are the same
string s;
typedef string *ps;
const ps      v1 = &s;
ps const      v2 = &s;
string *const v3 = &s;
```

### Pdf. 128

```cpp
#include<cstring>    // to use C-style string in C++

#include<string.h>   // to use C-style string in C

#include<string>     // to use std::string in C++
using std::string;
```

### Pdf. 131, 132

Create new `array`, initialized, and delete

```cpp
int *p = new int[10];  // create an array having 10 <int> elements

// const object must be initialized at definition
const int *p = new const int[10]();  // () : do value-initialization for internal type like int
const string *ps = new const string[10];  // initialized by invoking the default constructor of <string>

delete [] p;
delete [] ps;
```

### Pdf. 135

Use `c_str()` to get C-style string from `std::string`

```cpp
std::string s1 = "asfkskalf";
const char *ps = s1.c_str();  // c_str() : return C-style string, i.e., return a char* to the address of first element of char[], the char array ends with null (\0)
```

Initialized `std::vector` from `array`
```cpp
const size_t len = 6;
int a[len] = {1, 2, 3, 4, 5, 6};

std::vector<int> v(a, a+len);  // v is initialized as vector<int>{1,2,3,4,5,6}

std::vector<int> v(a+1, a+4);  // v is initialized as vector<int>{2,3, 4}
```

### Pdf. 138

Use `typedef` to simplify pointer to **multi-dimensional** `array`

```cpp
int a[3][4];
typedef int IntArray[4];
IntArray *p = a;  // p is initialized to pointing to the first element of a, which is a int[4] array
```

## Chapter-5 : Expression (Pdf. 141 / 769)

### Pdf. 142

* 操作数 operand
* 操作符 operator
    * unary (&, *)
    * binary (+, -, *)
    * ternary 

### Pdf. 152

The value of the assign expression is the value of its left operand, and the type of its result is the type of its left operand

### Pdf. 154

`++i` 
* return changed value, which is `i` itself
* a lvalue
* less work (+1 and return `i`)

i++

* return unchanged value, which is the original value of `i`
* a rvalue
* more work (save original, in order to return)

It is recommended to use `++i`, only use `i++` if really needed !!

### Pdf. 156

string s;
string *p = &s;
// equivalent 
(*p).size();
p->size();

### Pdf. 158
sizeof(expr) : sizeof can be applied to expression, expression will not be calculated

int a[10];
// sizeof(a)/sizeof(a[0]) == number of elements in a
int num = sizeof(a)/sizeof(*a)

### Pdf. 161, 162 

(p.147) Table 5-4 of priority of all operators

### Pdf. 164, 165, 166

About `new` expression

```c
int i(10);  // define int var i and initialize its value to 10
int *p = new int(10);  // p points to a int object whose value is initialized to 10
// int *p = new int[10];  // p is pointing to a int array object, whose size is 10

int *p = new int;  // p points to an uninitialized int object
int *p = new int(); // p points to an int object whose value is initialized to 0 (by default)

// Create const object dynamically
const int *p = new const int(10);  // must be initialized at creation. No more modification allowed
```

### Pdf. 170

In most cases, array will be converted to a pointer pointing to its first element

```c
int a[10];
int *p = a;
```

Except when
* & - address of
* sizeof
* initialize another array using this array

### Pdf. 172

Cast - `static_cast`, `dynamic_cast`, `const_cast`, `reinterpret_cast`

```c
double d = 9.0;
void *p = &d;
double *dp = static_cast<double*>(p); // convert back to double * type
```

### Pdf. 175

Old style type cast:

```c
int i; double d;
i += int(d);  // static_cast: convert double to int

const_char* cp;
string_copy((char *) cp);  // const_cast: cast away const

int *pi;
char *pc = (char *) ip;  // reinterpret_cast : treat int* as char*
```

## Chapter-6 : Statement (Pdf. 179 / 769)

### Pdf.202

Each standard Exception class defines a `what()` function, which returns C-style string

```c
try {
    ...
} catch (runtime_error err) {
    cout << err.what() << "\n"
         << "Try again?" << endl;
}
```

### Pdf.203

[p. 189] Table 6-1. Standard Exception Class defined in <stdexcept>

![](/assets/posts/2018-09-28-reading-notes__cpp-primer-4th/2018-09-28-reading-notes__cpp-primer-4t_2018-09-28-09-13-33.png)

### Pdf. 204

**Use pre-processor to debug**

Example

```c
int main() {
    #ifndef NDEBUG
    cout << "Starting main() ..." << endl;
    #endif
    // ....
}
```

* By default, `NDEBUG` not defined, so the output will be executed, which is convenient for debugging
* When to publish, run the following to delete those statement

```bash
$ gcc -DNDEBUG main.cpp
```
equivalent to 
```c
#define NDEBUG   // at the beginning of main,cpp
```

Other useful constant

* `__FILE__` : file name
* `__LINE__` : current line number
* `__TIME__` : compiling time
* `__DATE__` : compiling date

Example:

```c
if (capacity < threshold) {
    cerr << "Error: " << __FILE__
         << " : line " << __LINE__
         << " Compiled on " << __DATE__
         << " at " << __TIME__
         << " : capacity is too small !!" << endl;
}
```
