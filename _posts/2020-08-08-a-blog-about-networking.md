---
title: A Blog about Networking
date: 2020-08-08 16:00:00
tags: Network, IP, MAC
---

最近在想问题“IP与MAC的作用”，以及很早之前学过的网络知识。发现了一个非常好的网站 Practical Networking ([www.practicalnetworking.net/series](https://www.practicalnetworking.net/series))，所以记录一下这个网站以及我所学到的知识.

<!-- more -->

## 为什么要同时用到IP与MAC

先回顾一下OSI模型

* **Layer 1 Physical** - anything that carries 1’s and 0’s between two nodes.

* **Layer 2 Data Link** - group together 1’s and 0’s into chunks known as Frames.

* **Layer 3  Network** - responsible for packet delivery from end to end.

* **Layer 4 – Transport** - responsible for service to service delivery.

* **Layer 5, 6, and 7 - Session, Presentation, and Application** - handle the final steps before the data transferred through the network (facilitated by layers 1-4) is displayed to the end user.

在这其中， IP作用在Layer 3， MAC作用在Layer 2. 在数据的传输过程中，Source IP和Destination IP是一直不会变的。而Source MAC和Destination MAC地址会随着每一跳而改变。

换句话说

* **Layer 2 uses MAC addresses and is responsible for packet delivery from hop to hop.**

* **Layer 3 uses IP addresses and is responsible for packet delivery from end to end.**

其过程可以用这张图表示 ([来源](https://www.homenethowto.com/switching/mac-addresses/)):

![mac address traffic example](/assets/images/posts/2020/mac-address-traffic-example.jpg)