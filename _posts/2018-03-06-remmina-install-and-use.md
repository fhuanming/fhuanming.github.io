---
title: Remmina 从Ubuntu上远程登陆Windows
date: 2018-03-06 23:00:00
tags: Remmina Ubuntu Windows
---

上一篇文章 ["Samba 在Windows上访问Ubuntu文件"](../../../../2018/03/06/samba-install-and-use/) 介绍了如何在Widnows上像访问本地文件一样访问Ubuntu文件。这篇文章将介绍如何在Ubuntu上面远程登陆Windows.

<!-- more -->

# 在Windows上

1. 首先需要 Enable Remote Desktop Connection. 由于我是windows 8.1 Basic Edition, 没有远程登录的功能。所以这里我就需要安装 [RDP Wrapper](https://github.com/stascorp/rdpwrap). You can download the release [here](https://github.com/stascorp/rdpwrap/releases).

2. 运行 `intall.bat` 进行安装。

3. 运行 `RDPConf.exe`. 然后 uncheck `"single session per user"` 的选项。

4. 然后运行 `RDPCheck.exe` 确认设置无误。

# 在Ubuntu上

1. 安装 Remmina from Ubuntu Software store.

2. 点击 `New` 添加新的 Config.

3. 输入Windows的 Server (IP), User Name. Password.

4. 点击 `Connect`.