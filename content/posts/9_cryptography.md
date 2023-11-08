+++
title = "cryptography"
author = ["Dony Ridani"]
date = 2023-07-24T00:00:00+08:00
lastmod = 2023-09-17T17:54:31+08:00
tags = ["crypto"]
draft = false
+++

## common sense {#common-sense}

there are five solution to solve unser problems

1.  symmetric cryptography
2.  public-key cryptography
3.  hash
4.  Message Authentication Code
5.  Digital Signature

| threat    | speciality      | solution          |
|-----------|-----------------|-------------------|
| eavesdrop | Confidentiality | 1 &amp; 2         |
| falsity   | Integrity       | 3 &amp; 4 &amp; 5 |
| disguise  | Authenticity    | 4 &amp; 5         |
| denial    | Nonrepudiation  | 5                 |

shoule use public crypto algorithm, instead of private crypto algorithm


## cryptography in history {#cryptography-in-history}


### caesar cipher {#caesar-cipher}

just brute-force attack

```cpp
#include <cctype>
#include <iostream>
using namespace std;

void func(string s) {
  for (int i = 0; i < 26; ++i) {
    string temp(s);
    for (auto &c : temp) {
      c = tolower(c);
      c = ((c + i) - 97) % 26 + 97;
    }
    cout << temp << endl;
  }
}

int main() {
  string s;
  cin >> s;
  func(s);
}
```


### simple substitution cipher {#simple-substitution-cipher}

[Letter_frequency](https://en.wikipedia.org/wiki/Letter_frequency)


### Enigma {#enigma}

read the following books:

-   The Code Book: The Science of Secrecy from Ancient Egypt to Quantum Cryptography Paperback – August 29, 2000

-   Alan Turing: The Enigma: The Book That Inspired the Film The Imitation Game - Updated Edition Paperback – Illustrated, November 10, 2014

-   Code Breaking: A History and Explanation Paperback – December 4, 2012

To be continued, recently I found the book I am reading was too elementary, so I decide not to continue finish it, and I will search for some material cover the subject in enough depth.
