---
layout: post
title: Install Zend Server 7.0.0 and Mysql 5.6.20 on MacOSx 10.9.4
---

By default, Zend Server 7.0.0 has MySQL 5.5. But there are lots of feature in [MySQL 5.6.20](http://dev.mysql.com/doc/relnotes/mysql/5.6/en/). ANd i decide to upgrade MySQL to 5.5 to 5.6.20. First of all, i get a backup from **PhpMyAdmin**. This is very important. And shutdown all Zend Server with 

```
sudo /usr/local/zend/bin/zendctl.sh stop
```

And i open `zendctl.sh` file with nano and remove all mysql start and stop operation from there.

```
....

    "start-mysql")
        if $MYSQL_EN; then
                    $ZCE_PREFIX/mysql/bin/mysql.server start
                fi
        ;;

....
        
    "stop-mysql")
        if $MYSQL_EN; then
                    $ZCE_PREFIX/mysql/bin/mysql.server stop
                fi
        ;;
        
....

    "restart-mysql")
        if $MYSQL_EN;then
            $0 stop-mysql
            sleep 2
            $0 start-mysql
        fi
        ;;

```

I save and close file. After that, start the Zend Server and check Activity Monitor for MySQL. MySQL was not started. I completely delete files from `/usr/local/zend/mysql/`. 

Now, i install MySQL 5.6.20 from [http://dev.mysql.com/downloads/mysql/](http://dev.mysql.com/downloads/mysql/). Select your platform and DMG Archive. After downloading, install it with next. The installation directory of MySQL is `/usr/loca/mysql`. We are work in this directory. MySQL installer also includes a MySQL Preference Pane. You can start and stop your MySQL Server from MacOSx System Preferences Panel. 

Now, we change the `my.cnf` file for new mysql. I change it like this: 

```
[client]
#password   = your_password
port        = 3306
socket      = /tmp/mysql.sock

[mysqld]

port        = 3306
socket      = /tmp/mysql.sock
skip-external-locking
key_buffer_size = 16K
max_allowed_packet = 1M
table_open_cache = 4
sort_buffer_size = 64K
read_buffer_size = 256K
read_rnd_buffer_size = 256K
net_buffer_length = 2K
thread_stack = 128K

sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
```

After that, save the file and your can change every settings with the mysqladmin.

```
sudo mysqladmin status --socket=/tmp/mysql.sock -h 127.0.0.1 -P 3306 -u root -p
```

Then restart your MySQL Server. Now, we must change our php.ini file to connect from PHP. Your Zend Server php.ini file is `/usr/local/zend/etc/php.ini`. 

You must change the following lines : 

```
...
pdo_mysql.default_socket=/tmp/mysql.sock
...
mysql.default_socket=/tmp/mysql.sock
...
mysqli.default_socket=/tmp/mysql.sock
...
```

Now, You can restart Zend Server and  it is ready. 
