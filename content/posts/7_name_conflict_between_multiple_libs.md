+++
title = "name conflict multiple libs"
author = ["Dony Ridani"]
date = 2023-07-06T00:00:00+08:00
lastmod = 2023-09-17T17:53:10+08:00
tags = ["link"]
draft = false
+++

## Here is the Question {#here-is-the-question}

```c
/*A.c*/
#include <stdio.h>
int sayHi() {
  printf("Hi, this is AAAAA\n");
  return 0;
}
int sayOut() {
  sayHi();
  printf("Use this to introduce AAAAA\n");
  return 0;
}

```

```c
/*B.c*/
#include <stdio.h>
int sayHi() {
  printf("Hi, this is BBBBB\n");
  return 0;
}
int sayOut() {
  sayHi();
  printf("Use this to introduce BBBBB\n");
  return 0;
}

```

```c
#include <stdio.h>
extern int sayOut();

int main() {
  sayOut();
  return 0;
}

```

```bash
# if link A first
>gcc test.c -L. -lA -lB
>./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
# if link B first
>gcc test.c -L. -lB -lA
>./a.out
Hi, this is BBBBB
Use this to introduce BBBBB
```


## First solution {#first-solution}

at first i use dlopen to explicitly load a dynamic library

```c
#include <stdio.h>
#include<dlfcn.h>
extern int sayOut();

typedef int (*func_pt)();


int main() {
    void *handle = NULL;
    func_pt func = NULL;
    if((handle = dlopen("./libA.so",RTLD_LAZY))== NULL)
    {
        printf("dlopen %s\n", dlerror());
        return -1;
    }
    func = dlsym(handle, "sayOut");
    func();
    dlclose(handle);

    if((handle = dlopen("./libB.so",RTLD_LAZY))== NULL)
    {
        printf("dlopen %s\n", dlerror());
        return -1;
    }
    func = dlsym(handle, "sayOut");
    func();
    dlclose(handle);
  return 0;
}

```

```bash
❯gcc test.c -L. -lA -lB
❯ ./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
Hi, this is AAAAA
Use this to introduce BBBBB

❯ gcc test.c -L. -lB -lA
❯ ./a.out
Hi, this is BBBBB
Use this to introduce AAAAA
Hi, this is BBBBB
Use this to introduce BBBBB
```

we can see the outer func sayOut is load explicitly, however the inner function still related to linking order
it is because the inner function are exposed to the outside


## Second solution {#second-solution}

i use <span class="underline"><span class="underline">attrubute</span></span> to control if a function should expose to the outside
here is an example

```c
/*A.c*/
#include <stdio.h>
#define DLL_PUBLIC __attribute__((visibility("default")))
#define DLL_LOCAL __attribute__((visibility("hidden")))

DLL_LOCAL int sayHi() {
  printf("Hi, this is AAAAA\n");
  return 0;
}
DLL_PUBLIC int sayOut() {
  sayHi();
  printf("Use this to introduce AAAAA\n");
  return 0;
}

/*B.c*/
#include <stdio.h>

#define DLL_PUBLIC __attribute__((visibility("default")))
#define DLL_LOCAL __attribute__((visibility("hidden")))

DLL_LOCAL int sayHi() {
  printf("Hi, this is BBBBB\n");
  return 0;
}
DLL_PUBLIC int sayOut() {
  sayHi();
  printf("Use this to introduce BBBBB\n");
  return 0;
}

```

```bash
❯ gcc test.c -L. -lA -lB
❯ ./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
Hi, this is BBBBB
Use this to introduce BBBBB

❯ gcc test.c -L. -lB -lA
❯ ./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
Hi, this is BBBBB
Use this to introduce BBBBB
```


## Third solution {#third-solution}

The third way is only set outer function to visible
and compile it with -fvisibility=hidden argument

```c
/*A.c*/
#include <stdio.h>
#define DLL_PUBLIC __attribute__((visibility("default")))

int sayHi() {
  printf("Hi, this is AAAAA\n");
  return 0;
}
DLL_PUBLIC int sayOut() {
  sayHi();
  printf("Use this to introduce AAAAA\n");
  return 0;
}

/*B.c*/
#include <stdio.h>

#define DLL_PUBLIC __attribute__((visibility("default")))

int sayHi() {
  printf("Hi, this is BBBBB\n");
  return 0;
}
DLL_PUBLIC int sayOut() {
  sayHi();
  printf("Use this to introduce BBBBB\n");
  return 0;
}
```

```bash
and it worked as expected
❯ gcc test.c -L. -lA -lB
❯ ./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
Hi, this is BBBBB
Use this to introduce BBBBB

❯ gcc test.c -L. -lB -lA
❯ ./a.out
Hi, this is AAAAA
Use this to introduce AAAAA
Hi, this is BBBBB
Use this to introduce BBBBB
```

[https://blog.csdn.net/giveaname/article/details/103353828](https://blog.csdn.net/giveaname/article/details/103353828#:~:text=%E5%8E%9F%E5%9B%A0%E5%9C%A8%E4%BA%8E%E5%8A%A8%E6%80%81%E5%BA%93%E4%B8%AD%E7%9A%84%E5%86%85%E9%83%A8%E5%87%BD%E6%95%B0%E6%B2%A1%E6%9C%89%E8%AE%BE%E7%BD%AE%E9%99%90%E5%88%B6%EF%BC%8C%E4%BD%BF%E5%BE%97sayHi%E5%87%BD%E6%95%B0%E4%B9%9F%E6%9A%B4%E9%9C%B2%E7%BB%99%E5%A4%96%E9%83%A8%EF%BC%8C%E8%B0%83%E7%94%A8%E6%97%B6%E8%87%AA%E7%84%B6%E9%80%89%E6%8B%A9%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0%E3%80%82,%E7%94%A8nm%E6%8C%87%E4%BB%A4%E5%8F%AF%E4%BB%A5%E7%9C%8B%E5%87%BA%EF%BC%8C%E4%B8%A4%E4%B8%AA%E5%87%BD%E6%95%B0%E9%83%BD%E6%9A%B4%E9%9C%B2%E5%87%BA%E6%9D%A5%E4%BA%86%20%E5%86%8D%E6%AC%A1%E6%90%9C%E7%B4%A2%EF%BC%8C%E5%8F%AF%E4%BB%A5%E7%94%A8gcc%E7%BC%96%E8%AF%91%E5%99%A8%E7%9A%84%E7%89%B9%E6%80%A7%E6%9D%A5%E8%AE%BE%E7%BD%AE%E5%8A%A8%E6%80%81%E5%BA%93%E5%87%BD%E6%95%B0%E7%9A%84%E5%AF%BC%E5%87%BA%E6%8E%A7%E5%88%B6%E3%80%82)
