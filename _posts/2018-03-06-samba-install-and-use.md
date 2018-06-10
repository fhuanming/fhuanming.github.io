---
title: Samba 在Windows上访问Ubuntu文件
date: 2018-03-06 22:00:00
tags: Samba Ubuntu Windows
---

最近新买了一个主机，装了Ubuntu系统，于是我就有了Windows和Ubuntu两台电脑。打算将Ubuntu用于Develop Environment. 将Windows用于生活。但是由于两台电脑（其实是一个laptop和一个intel NUC）连接的一个显示器，鼠标和键盘也只有一套，在来回切换的时候很不方便，于是便琢磨起如何将Ubuntu和Windows相互连接到一起。

这篇文章介绍的是如何用Windows作为主机，访问Ubuntu上的文件的方法。下篇文章 ["Remmina 从Ubuntu上远程登陆Windows"](../../../../2018/03/06/remmina-install-and-use/) 将介绍如何用Ubuntu作为主机，连接到Windows上。

<!-- more -->

# 在Ubuntu上

首先安装Samba
{% highlight shell %}
$ sudo apt-get install samba
{% endhighlight %}

备份初始的config
{% highlight shell %}
$ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
{% endhighlight %}

在config里面添加如下内容
{% highlight shell %}
$ vim /etc/samba/smb.conf
{% endhighlight %}

{% highlight linux-config %}
[the_name_shows_on_windows]
    path = /the/dir/you/want/to/map/to/windows
    available = yes
    valid users = your_user_name
    writeable = yes
    browsable = yes
    read only = no
{% endhighlight %}
更多关于config的设定，请看这里 [samba.org](https://www.samba.org/samba/docs/current/man-html/smb.conf.5.html).

为你刚刚在config里面添加的user设置密码
{% highlight shell %}
$ sudo smbpasswd -a fhuanming
{% endhighlight %}

重起服务
{% highlight shell %}
$ sudo service smbd restart
{% endhighlight %}

# 在Windows上

进入我的电脑，右健 Add Network Location.根据引导，最后输入
{% highlight shell %}
\\Your_Ubuntu_Hostname\the_name_shows_on_windows
{% endhighlight %}

自此你就可以像访问本地的文件一样访问Ubuntu的文件了。