---
layout: post
title: g++ Preprocessor Output
category: Cpp
tags: Cpp
keywords: Preprocess 
published: true
---

# g++ Preprocessor Output

## Create Preprocessor Output

Run the following command, and you can see the output from the preprocessor of g++
* All `#include`, `#define`, `#if`, `#ifdef` are removed and replaced with the actual value
* Any line starts with # tells 
    * line number : the first field
    * file name : the 2nd field
    * flag : the 3rd / 4th field(s), See the link for details about the flag: <https://gcc.gnu.org/onlinedocs/cpp/Preprocessor-Output.html>
        * 1 - start of a new file 
        * 2 - return to a file (after #include)
        * 3 - following from a system header file
        * 4 - following having an implicit `extern "C"` 

```bash
g++ -E test.cpp
```

```cpp
#include <stdio.h>
int main() {
    const size_t as = 10;
    return as;
}
```

## Check pre-defined macros in <built-in>

```bash
cpp  -dM foo.h
```
Or
```bash
g++  -E  -dM foo.h
```
* Note: `foo.h` is a empty dummy file

Output

```
#define OBJC_NEW_PROPERTIES 1
#define _LP64 1
#define __APPLE_CC__ 6000
#define __APPLE__ 1
...
#define __BLOCKS__ 1
#define __BYTE_ORDER__ __ORDER_LITTLE_ENDIAN__
#define __CHAR16_TYPE__ unsigned short
#define __CHAR32_TYPE__ unsigned int
#define __CHAR_BIT__ 8
...
#define __INT16_MAX__ 32767
#define __INT16_TYPE__ short
...
#define __INT32_MAX__ 2147483647
#define __INT32_TYPE__ int
...
#define __INT64_MAX__ 9223372036854775807LL
#define __INT64_TYPE__ long long int
...
#define __INT8_MAX__ 127
#define __INT8_TYPE__ signed char
...
#define __INTMAX_MAX__ 9223372036854775807L
#define __INTMAX_TYPE__ long int
...
#define __INTPTR_MAX__ 9223372036854775807L
#define __INTPTR_TYPE__ long int
...
#define __SIZEOF_DOUBLE__ 8
#define __SIZEOF_FLOAT__ 4
#define __SIZEOF_INT128__ 16
#define __SIZEOF_INT__ 4
#define __SIZEOF_LONG_DOUBLE__ 16
#define __SIZEOF_LONG_LONG__ 8
#define __SIZEOF_LONG__ 8
#define __SIZEOF_POINTER__ 8
#define __SIZEOF_PTRDIFF_T__ 8
#define __SIZEOF_SHORT__ 2
#define __SIZEOF_SIZE_T__ 8
#define __SIZEOF_WCHAR_T__ 4
#define __SIZEOF_WINT_T__ 4
#define __SIZE_FMTX__ "lX"
#define __SIZE_FMTo__ "lo"
#define __SIZE_FMTu__ "lu"
#define __SIZE_FMTx__ "lx"
#define __SIZE_MAX__ 18446744073709551615UL
#define __SIZE_TYPE__ long unsigned int
#define __SIZE_WIDTH__ 64
...
#define __USER_LABEL_PREFIX__ _
#define __VERSION__ "4.2.1 Compatible Apple LLVM 10.0.0 (clang-1000.10.44.2)"
#define __WCHAR_MAX__ 2147483647
#define __WCHAR_TYPE__ int
#define __WCHAR_WIDTH__ 32
#define __WINT_MAX__ 2147483647
#define __WINT_TYPE__ int
#define __WINT_WIDTH__ 32
...
#define __cplusplus 199711L
#define __cpp_exceptions 199711
#define __cpp_rtti 199711
#define __cpp_threadsafe_static_init 200806
#define __llvm__ 1
#define __nonnull _Nonnull
#define __null_unspecified _Null_unspecified
#define __nullable _Nullable
#define __pic__ 2
#define __private_extern__ extern
#define __strong 
#define __tune_core2__ 1
#define __unsafe_unretained 
#define __weak __attribute__((objc_gc(weak)))
#define __x86_64 1
#define __x86_64__ 1
```
