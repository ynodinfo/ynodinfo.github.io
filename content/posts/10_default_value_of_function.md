+++
title = "Default value of function"
author = ["Dony Ridani"]
date = 2023-07-27T00:00:00+08:00
lastmod = 2023-09-17T17:55:10+08:00
tags = ["c"]
draft = false
+++

Should I set default value when write a function, and where to set? In the declaration or the implementation
the answer is in the declaration, and should in header file. In case of other part uses this function and not set a default value, if set a default value in the implementation, the other file included this file can't know what default value is, and not able to compile it.
For example

```cpp
//*.h
int add(int a, int b);
//*.cc
int add(int a = 1, int b = 1) { return a + b; }
// test.cc
#include "a.h"
#include <iostream>
using namespace std;
int main() {
  cout << add(1) << endl;
  ;
}
```

```bash
❯ g++ test.cc -L. -la
test.cc: In function ‘int main()’:
test.cc:6:16: error: too few arguments to function ‘int add(int, int)’
    6 |     cout << add(1)<< endl;;
      |             ~~~^~~
In file included from test.cc:1:
a.h:1:5: note: declared here
    1 | int add(int a , int b );
      |     ^~~
```

in the above program, test.cc include a.h, but default value is not set in the header file, when compile test.cc, the compiler will require add function two parameters, but only one is here, so it will not be able to compile.
