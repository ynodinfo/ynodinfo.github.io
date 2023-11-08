+++
title = "SICP"
author = ["Dony Ridani"]
date = 2023-08-06T00:00:00+08:00
lastmod = 2023-09-17T17:55:52+08:00
tags = ["scheme", "sicp"]
draft = false
+++

## Building Abstractions with Procedures {#building-abstractions-with-procedures}


### At first this book talk about elements of programming is interesting {#at-first-this-book-talk-about-elements-of-programming-is-interesting}

-   primitive expressions
-   means of combination
-   means of abstraction

in this book we are going to use scheme language, and I use chicken interpreter, just because I like it's interesting name.

```scheme
this language use prefix notation like this
(+ 1 2 3 4 5 6 7 8 9)
name something like value
(define size 2)
this associate the value 2 with the name size

define procedure
(define (suqare x) (* x x))
or
(define square (lambda (x) (* x x))
this is some kind of syntactic sugar, but the lambda type is more essential.

a good question asked at the end of first class, what's the difference between the following two expressions
(define a (* 5 5 ))
(define (b) (* 5 5))
 a  ==> 25
(a) ==> Error: call of non-procedure: 25
 b  ==> #<procedure (b)>
(b) ==> 25
```


### applicative order versus normal order {#applicative-order-versus-normal-order}

"fully expand and then reduce" method is normal-order, and "evaluate the arguments and then apply" method is applicative-order evaluation, and the second method is the interpreter actually uses. partly because of the additional efficiency obtained from avoiding multiple evaluation of expressions, and more significantly, normal-order evaluation becomes much more complicated to deal with when we leave the realm of procedures that can be modeled by substitution.

if you can name it, you have power of it
