---
title: Linux Directory For Software
date: 2019-05-02 22:00:00
tags: Linux
---

用Windows的可能都知道，软件一般会被安装在C盘的`Program Files`或者`Program Files (x86)`目录下。那么在Linux上有没有对应的目录呢？

答案是没有。通常软件会被安装在`/bin`, `/usr/bin`, `/usr/share`, `/usr/local`或者`/opt`下。

那么对于一个已经安装了的软件，该如何寻找它的安装地址呢？这个[答案](https://askubuntu.com/a/294492)给了很好的解释。简而言之，可以用`type`, `which`或者`whereis` cmd去查询。除此之外，这个[答案](https://askubuntu.com/a/551932)解释了每个目录存放的大致的内容。
