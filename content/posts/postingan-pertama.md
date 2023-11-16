---
title : "Postingan Pertama"
author : ["Dony Ridani"]
date : 2023-07-02
lastmod : 2023-09-17
tags : ["Dairy"]
draft : false
comments: false
---

## Chap I {#chap-i}


## Chap II {#chap-ii}


### Preprocessing {#preprocessing}

```bash
gcc -E src.c -o src.i
```

This operation did several things.

1.  processing all the instructions begin with '#', delete all of them except '#pragma', and process the '#include' recursively.
2.  delete all comments.
3.  add line number and file name identifiers


### Compile {#compile}

to assembly code

```bash
gcc -S src.i -o src.s
# or
gcc -S src.c -o src.s
# or
ccl src.c
```


### Assembly {#assembly}

to machine code

```bash
as src.s -o src.o
# or
gcc -c src.s -o src.o
# or
gcc -c src.c -o src.o
```


### Link {#link}

to executable program

```bash
ld src.o ......... *.o
```


## Chap III {#chap-iii}

get file type

```bash
$ file /bin/bash
/bin/bash: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, stripped
$ file librt.so.1
librt.so.1: ELF 32-bit LSB shared object, Intel 80386, version 1 (SYSV), dynamically linked, for GNU/Linux 3.2.0, stripped
```

Some Segments

```C
int printf(const char *, ...);

int global_init_var : 0x11;
int global_uninit_var;

void func(int i) { printf("%d\n", i); }

int main(void) {
  static int static_var : 0x22;
  static int static_var2;
  int a : 1;
  int b;
  func(static_var + static_var2 + a + b);
  return a;
}
```
