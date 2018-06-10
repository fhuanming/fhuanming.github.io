---
title: 使用SSH 从Windows连接到Ubuntu
date: 2018-03-07 20:30:00
tags: SSH Ubuntu Windows
---

前两篇文章分别介绍过

* 如何在Windows上访问Ubnuntu的文件 ["Samba 在Windows上访问Ubuntu文件"](../../../../2018/03/06/samba-install-and-use/).

* 如何在Ubuntu上面远程登录Windows ["Remmina 从Ubuntu上远程登陆Windows"](../../../../2018/03/06/remmina-install-and-use/).

这篇文章要介绍通过SSH, 从Windows上登陆Ubuntu.

<!-- more -->

# 在Ubuntu上

首先安装并开启SSH Server:
{% highlight shell %}
$ sudo apt-get install openssh-server
{% endhighlight %}

# 在Windows上

首先下载安装Cygwin, 注意安装的时候也要勾选SSH模块。

然后生成Key Pair:
{% highlight shell %}
$ ssh-keygen
{% endhighlight %}

将Public Key拷贝到Ubuntu上面
{% highlight shell %}
$ scp id_rsa.pub your_user_name@${ubuntu_ip}:/tmp/id_rsa.pub
{% endhighlight %}

第一次先用密码通过SSH登陆到Ubuntu
{% highlight shell %}
$ ssh your_user_name@${ubuntu_ip}
{% endhighlight %}

然后将Public Key拷贝到authorized_keys
{% highlight shell %}
$ cat /tmp/id_rsa.pub >> ~/.ssh/authorized_keys
{% endhighlight %}

最后修改SSH config. 这样就可以使用Key Pair
登陆了，不需要每次再输入密码了。
{% highlight shell %}
$ sudo vim /etc/ssh/sshd_config
{% endhighlight %}

将 `PasswordAuthentication yes` 改为 `PasswordAuthentication no`.

重起SSH Server.
{% highlight shell %}
$ service ssh restart
{% endhighlight %}
