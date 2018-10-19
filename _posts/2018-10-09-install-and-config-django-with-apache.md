---
title: Install and Config Django with Apache
date: 2018-10-09 20:30:00
tags: Django Apache mod_wsgi wsgi Ubuntu python virtualenv
---

这篇文章要介绍在Ubuntu上面如何从零开始配置Django和Apache. 要说明的一点是我这里使用的是python3.

<!-- more -->

# Install pip

{% highlight shell %}
$ sudo apt-get install python3-pip.
{% endhighlight %}

# Install virtualenv

{% highlight shell %}
$ pip3 install virtualenv
$ virtualenv ./.virtualenv/mysite -p python3
$ source .virtualenv/mysite/bin/activate
{% endhighlight %}

To exit the virtual env:

{% highlight shell %}
$ deactivate
{% endhighlight %}

# Install Apache

{% highlight shell %}
$ sudo apt-get install apache2
{% endhighlight %}

Some useful comments to restart apache service:

{% highlight shell %}
$ apachectl stop
$ apachectl start
$ apachectl restart
{% endhighlight %}

# Install mod_wsgi

{% highlight shell %}
$ sudo apt-get install libapache2-mod-wsgi-py3
{% endhighlight %}

# Install Django

Under the virtual env:
{% highlight shell %}
$ pip install Django
$ python -m django --version
{% endhighlight %}

# Create your first Django project and config it

{% highlight shell %}
$ django-admin startproject mysite
{% endhighlight %}

Go to `mysite/settings.py`. Add your host ip to `ALLOWED_HOSTS`:

```python
ALLOWED_HOSTS = [
    '35.232.xxx.xxx'
]
```

# Apache Config

## If you want to config the mod_wsgi as embedded mode  

Go to Apache config. For Ubuntu the file path is `/etc/apache2/apache2.conf`. For other systems, please check [Apache website](https://wiki.apache.org/httpd/DistrosDefaultLayout#Apache_httpd_2.4_default_layout_.28apache.org_source_package.29). Add following config into the file:

```config
WSGIScriptAlias / /home/yourusername/mysite/mysite/wsgi.py
WSGIPythonHome /home/yourusername/.virtualenv/mysite
WSGIPythonPath /home/yourusername/mysite

<Directory /home/yourusername/mysite/mysite>
<Files wsgi.py>
Require all granted
</Files>
</Directory>
```

## If you want to config the mod_wsgi as deamon mode

Add a new Apache VirtualHost config file (you can also modify the exsiting default file). For Ubuntu the file path is under `/etc/apache2/sites-available`. For other systems, please check [Apache website](https://wiki.apache.org/httpd/DistrosDefaultLayout#Apache_httpd_2.4_default_layout_.28apache.org_source_package.29):

{% highlight shell %}
$ sudo vim /etc/apache2/sites-available/mysite.conf
{% endhighlight %}

Then you can add following config into the file:

```config
<VirtualHost *:80>
  WSGIDaemonProcess mysite processes=2 threads=15 display-name=%{GROUP} python-home=/home/fhuanming/.virtualenv/mysite python-path=/home/fhuanming/mysite

  WSGIProcessGroup mysite
  WSGIApplicationGroup %{GLOBAL}
  
  WSGIScriptAlias / /home/fhuanming/mysite/mysite/wsgi.py

  <Directory /home/fhuanming/mysite/mysite>
    <Files wsgi.py>
      Require all granted
    </Files>
  </Directory>
</VirtualHost>
```

Then disable the default VirtualHost config and enable your new added.

```ShellSession
$ sudo a2dissite 000-default.conf
$ sudo systemctl reload apache2
$ sudo a2ensite mysite
$ sudo systemctl reload apache2
```

# Check your website

Then go to your browser to visit `http://35.232.xxx.xxx:80`. You will find your config works. :D