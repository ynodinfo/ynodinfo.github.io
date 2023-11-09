+++
title = "Advantages using Gentoo"
description = "when I install Emacs, and I need Emacs Lisp native compiler support, I just need to add a USE flag like below"
author = ["Dony Ridani"]
date = 2023-07-04T00:00:00+08:00
lastmod = 2023-09-17T17:53:51+08:00
tags = ["linux", "gentoo"]
draft = false
+++

After using several Linux distributions, it hard for me to find one suit me well, I have been using openSUSE TW for about two years, it is an excellent system, help me and take care of all the miscellaneous. But when I started getting involved with Linux Kernel, I would like to try some interesting features like the musl libc and no-multilib system, and it is easy to customize my own system from kernel to software. If a want a feature I can tell the system I need it and recompile the software, of course other system can do that too, but Gentoo offers the most convenient way to do such thing through it's "USE flag".

For example, when I install Emacs, and I need Emacs Lisp native compiler support, I just need to add a USE flag like below

```nil
app-editors/emacs jit
```

then recompile the Emacs, I will get an Emacs with Emacs Lisp native compiler support. Without a doubt it is very easy to customize software.
