+++
title = "Build IDE with emacs"
author = ["Dony Ridani"]
date = 2023-07-05T00:00:00+08:00
lastmod = 2023-09-17T17:51:41+08:00
tags = ["emacs"]
draft = false
+++

I am not recommended to build from scratch, there are several community driven configuration framework, like Doom Emacs Spacemacs... etc. I am highly recommend to use [Doom Emacs](https://github.com/doomemacs/doomemacs), it's very easy to install, just follow it's [Getting Started Guide](https://github.com/doomemacs/doomemacs/blob/master/docs/getting_started.org)
After install it, open it and type spc+f+p to open config file, int the init.el ,we can Remove comments to enable some feature(make sure to execute doom sync to install the feature), like cc, python, and so many tools that can help us to do many things.

Recently I found a useful tool that can help me to correct my word spell and grammar mistake, it called grammar in the init.el's checkers category. After all set up well, execute the langtool-check command in emacs to check the grammar and then execute langtool-correct-buffer to correct the grammar mistake.
