---
title: Connect Django and MySQL
date: 2018-11-01 20:30:00
tags: Django MySQL Ubuntu
---

[上一篇文章](../../../../2018/03/06/install-and-config-django-with-apache/)介绍了如何Django和Apache的安装与配置．这篇文章将介绍如何连接Django和MySQL.

<!-- more -->

# Install MySql

```console
sudo apt-get update
sudo apt-get install mysql-server
```

Here I met a error said

```console
update-alternatives: using /etc/mysql/mysql.cnf to provide /etc/mysql/my.cnf (my.cnf) in auto mode
Renaming removed key_buffer and myisam-recover options (if present)
Initialization of mysqld failed: 0
Warning: Unable to start the server.
Created symlink /etc/systemd/system/multi-user.target.wants/mysql.service → /lib/systemd/system/mysql.service.
Job for mysql.service failed because the control process exited with error code.
See "systemctl status mysql.service" and "journalctl -xe" for details.
invoke-rc.d: initscript mysql, action "start" failed.
● mysql.service - MySQL Community Server
   Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
   Active: activating (auto-restart) (Result: exit-code) since Tue 2018-10-30 03:26:34 UTC; 20ms ago
  Process: 27490 ExecStart=/usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid (code=exited, status=1/FAILURE)
  Process: 27469 ExecStartPre=/usr/share/mysql/mysql-systemd-start pre (code=exited, status=0/SUCCESS)

Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Failed with result 'exit-code'.
Oct 30 03:26:35 instant-1 systemd[1]: Failed to start MySQL Community Server.
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Service hold-off time over, scheduling restart.
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Scheduled restart job, restart counter is at 2.
Oct 30 03:26:35 instant-1 systemd[1]: Stopped MySQL Community Server.
Oct 30 03:26:35 instant-1 systemd[1]: Starting MySQL Community Server...
Oct 30 03:26:35 instant-1 mysqld[27568]: Initialization of mysqld failed: 0
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Control process exited, code=exited status=1
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Failed with result 'exit-code'.
Oct 30 03:26:35 instant-1 systemd[1]: Failed to start MySQL Community Server.
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Service hold-off time over, scheduling restart.
Oct 30 03:26:35 instant-1 systemd[1]: mysql.service: Scheduled restart job, restart counter is at 3.
Oct 30 03:26:35 instant-1 systemd[1]: Stopped MySQL Community Server.
Oct 30 03:26:35 instant-1 systemd[1]: Starting MySQL Community Server...
Oct 30 03:26:36 instant-1 mysqld[27610]: Initialization of mysqld failed: 0
Oct 30 03:26:36 instant-1 systemd[1]: mysql.service: Control process exited, code=exited status=1
Oct 30 03:26:36 instant-1 systemd[1]: mysql.service: Failed with result 'exit-code'.
Oct 30 03:26:36 instant-1 systemd[1]: Failed to start MySQL Community Server.
Oct 30 03:26:36 instant-1 systemd[1]: mysql.service: Service hold-off time over, scheduling restart.
Oct 30 03:26:36 instant-1 systemd[1]: mysql.service: Scheduled restart job, restart counter is at 4.
Oct 30 03:26:36 instant-1 systemd[1]: Stopped MySQL Community Server.
Oct 30 03:26:36 instant-1 systemd[1]: Starting MySQL Community Server...
dpkg: error processing package mysql-server-5.7 (--configure):
 installed mysql-server-5.7 package post-installation script subprocess returned error exit status 1
dpkg: dependency problems prevent configuration of mysql-server:
 mysql-server depends on mysql-server-5.7; however:
  Package mysql-server-5.7 is not configured yet.

dpkg: error processing package mysql-server (--configure):
 dependency problems - leaving unconfigured
Processing triggers for libc-bin (2.27-3ubuntu1) ...
Processing triggers for ureadahead (0.100.0-20) ...
No apport report written because the error message indicates its a followup error from a previous failure.
          Processing triggers for systemd (237-3ubuntu10.3) ...
Errors were encountered while processing:
 mysql-server-5.7
 mysql-server
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

After some search and debug, [this](https://askubuntu.com/questions/766038/error-installing-mysql-on-ubuntu-16-04) solved my problem. Basically, run:

```console
tail -n 20 /var/log/mysql/error.log
```

And I found

```console
2018-10-30T03:26:36.780349Z 0 [Note] InnoDB: Mutexes and rw_locks use GCC atomic builtins
2018-10-30T03:26:36.780353Z 0 [Note] InnoDB: Uses event mutexes
2018-10-30T03:26:36.780357Z 0 [Note] InnoDB: GCC builtin __atomic_thread_fence() is used for memory barrier
2018-10-30T03:26:36.780360Z 0 [Note] InnoDB: Compressed tables use zlib 1.2.11
2018-10-30T03:26:36.780363Z 0 [Note] InnoDB: Using Linux native AIO
2018-10-30T03:26:36.780706Z 0 [Note] InnoDB: Number of pools: 1
2018-10-30T03:26:36.780844Z 0 [Note] InnoDB: Using CPU crc32 instructions
2018-10-30T03:26:36.791352Z 0 [Note] InnoDB: Initializing buffer pool, total size = 128M, instances = 1, chunk size = 128M
2018-10-30T03:26:36.791404Z 0 [ERROR] InnoDB: mmap(137428992 bytes) failed; errno 12
2018-10-30T03:26:36.791412Z 0 [ERROR] InnoDB: Cannot allocate memory for the buffer pool
2018-10-30T03:26:36.791417Z 0 [ERROR] InnoDB: Plugin initialization aborted with error Generic error
2018-10-30T03:26:36.791426Z 0 [ERROR] Plugin 'InnoDB' init function returned error.
2018-10-30T03:26:36.791431Z 0 [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
2018-10-30T03:26:36.791438Z 0 [ERROR] Failed to initialize builtin plugins.
2018-10-30T03:26:36.791441Z 0 [ERROR] Aborting

2018-10-30T03:26:36.807276Z 0 [Note] Binlog end
2018-10-30T03:26:36.807371Z 0 [Note] Shutting down plugin 'CSV'
2018-10-30T03:26:36.807740Z 0 [Note] /usr/sbin/mysqld: Shutdown complete
```

Then I went to `/etc/mysql/mysql.conf.d/mysqld.cnf`, added `innodb_buffer_pool_size = 20M` under line `[mysqld]`. Then reinstall the mysql-server again. Then improve mySQL installation security by running

```console
sudo mysql_secure_installation
```

some useful cmd for sql:

```console
mysql -u db_user -p
show databases;
use %{database_name}
show tables;
select * from %{table_name}
```

# Create database and MySQL account for Django Application

Run following cmd to create database and user.

```console
CREATE DATABASE mysite_db CHARACTER SET UTF8;
CREATE USER mysite_user@localhost IDENTIFIED BY 'some_password';
GRANT ALL PRIVILEGES ON mysite_db.* TO mysite_user@localhost;
FLUSH PRIVILEGES;
```

# Install mysqlclient

Run following cmd to install mysqlclient.

```console
sudo apt-get install libmysqlclient-dev
pip install mysqlclient
```

# Connecting to the database

Modify the settings.py of the Django project with following

```shell
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mysite_db',
        'USER': 'mysite_user',
        'PASSWORD': 'some_password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```

Then run the cmd to create tables.

```console
python manage.py migrate
```
