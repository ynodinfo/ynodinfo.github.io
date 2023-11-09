+++
title = "What is enable shared from this in cpp"
author = ["Dony Ridani"]
date = 2023-07-03T00:00:00+08:00
lastmod = 2023-09-17T17:49:55+08:00
tags = ["cpp"]
description = "It is because when a shared_ptr was created, the control block was build up too, and there are three methods can create control block"
draft = false
+++

## Problem {#problem}

```cpp
class C : public enable_shared_from_this<C> {
public:
 std::shared_ptr<C>  func() {
    std::shared_ptr<C> local_sp_a(this);
    return local_sp_a;
  }
};

int main() {
    C c;
    auto a = c.func();
    return 0;
}
// Output:
// ./a.out
// double free or corruption (out)
// fish: Job 1, './a.out' terminated by signal SIGABRT (Abort)
```


## Why {#why}

it is because when a shared_ptr was created, the control block was build up too, and there are three methods can create control block

1.  make_shared
2.  make a shared_ptr from a unique_ptr
3.  make a shared_ptr from an origin pointer(this(class) is an origin pointer)

    Where there is more than one control block for only one object, it will free the object more than once.

    In order to get shared_ptr for a "this" pointer in a class, we need to use shared_from_this() to create shared_ptr point to Current object and connect to the same created control block, Avoided the multi control block problem


## Best practice {#best-practice}

```cpp
#include <iostream>
#include <memory>

using namespace std;

class C : public enable_shared_from_this<C> {
public:
  shared_ptr<C> GetThis() { return shared_from_this(); }
  static shared_ptr<C> Instance() { return shared_ptr<C>(new C()); }

private:
  C() = default;
};

int main() {
  shared_ptr<C> c = C::Instance();
  shared_ptr<C> a = c->GetThis();
  return 0;
}
```
