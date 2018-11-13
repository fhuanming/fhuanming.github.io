---
title: The Login of Root User on MySql
date: 2018-11-12 21:00:00
tags: Mysql Ubuntu root localhost plugin auth_socket authentication_string
---

最近发现使用`mysql -u root -p`登录MySql的时候会遇到`ERROR 1698 (28000): Access denied for user 'root'@'localhost'`错误。 但是`UPDATE user SET Password=PASSWORD('my_password') where USER='root';`并不能解决我的问题。经过一番调查发现是我`plugin`的设置的问题。

<!-- more -->

首先使用`sudo mysql -u root`登录MySql. 然后运行

```console
mysql> select user, Host, plugin, authentication_string from mysql.user where user="root";
+------+-----------+-------------+-----------------------+
| user | Host      | plugin      | authentication_string |
+------+-----------+-------------+-----------------------+
| root | localhost | auth_socket |                       |
+------+-----------+-------------+-----------------------+
```

你可以发现`plugin`显示的是`auth_socket`，而且`authentication_string`是空值。这是因为我在安装MySql的时候没有设置root. 导致了MySql可以以root和空密码的形式登录。而当我运行`UPDATE user SET Password=PASSWORD('my_password') where USER='root';`的时候，由于`plugin`是`auth_socket`，所以这条命令并没有起作用。

我们可以来看看普通的user这几项的值是什么

```console
mysql> select user, Host, plugin, authentication_string from mysql.user where user="your_non_root_user";
+-------------------------+-----------+-----------------------+-------------------------------------------+
| user                    | Host      | plugin                | authentication_string                     |
+-------------------------+-----------+-----------------------+-------------------------------------------+
| your_non_root_user      | localhost | mysql_native_password | *SOMEVALUE******************************* |
+-------------------------+-----------+-----------------------+-------------------------------------------+
```

你可以发现这里`plugin`的值是`mysql_native_password`. 所以只需要将root的`plugin`设置为`mysql_native_password`即可解决问题。

```console
mysql> update mysql.user set plugin='mysql_native_password' where User='root';
mysql> flush privileges;
```

对于`auth_socket`和`mysql_native_password`更详细的解释，可以参考[这里](https://www.percona.com/blog/2016/03/16/change-user-password-in-mysql-5-7-with-plugin-auth_socket/)。