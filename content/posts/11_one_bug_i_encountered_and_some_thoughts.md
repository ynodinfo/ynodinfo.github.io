+++
title = "One bug I encountered and some thoughts"
author = ["Dony Ridani"]
date = 2023-07-30T00:00:00+08:00
lastmod = 2023-09-17T17:55:39+08:00
tags = ["c", "c++"]
draft = false
+++

I met a bug recently, it's because I didn't notice the return value of a system call, and use it directly, sadly the compiler not be able to identify this kind problem, until I run it, I notice the program will never enter one condition.

finally I found the getenv system call returns char\* and I compare it with a string in C++, so this will not equal forever.

This reminds me a book called Algorithms + Data Structures = Programs. I must know what data structure I manipulated with. Under this promise, I will write good and correct programs. So deal with function input well is a good habit with doubt.
