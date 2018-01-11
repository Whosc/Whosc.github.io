---
title: php7 安装imap 扩展
date: 2018-01-11
tags: [运维,php]
categories:
- 技术
- 运维

---


### 安装 php-imap
 网上安装php7-imap 扩展的教程大多都太老了，不适用与现在在php7的版本，走了不少弯路，希望能帮到看到的人。

### php -v //查看当前php的版本
```
[root@vultr ~]# php -v
PHP 7.1.12 (cli) (built: Nov 27 2017 11:01:12) ( NTS )
Copyright (c) 1997-2017 The PHP Group
Zend Engine v3.1.0, Copyright (c) 1998-2017 Zend Technologies
    with Zend OPcache v7.1.12, Copyright (c) 1999-2017, by Zend Technologies
```

### 查看已经安装的扩展  yum list installed | grep php
```
[root@vultr ~]# yum list installed | grep php
mod_php71u.x86_64                   7.1.12-1.ius.centos7               @ius     
php71u-bcmath.x86_64                7.1.12-1.ius.centos7               @ius     
php71u-cli.x86_64                   7.1.12-1.ius.centos7               @ius     
php71u-common.x86_64                7.1.12-1.ius.centos7               @ius     
php71u-devel.x86_64                 7.1.12-1.ius.centos7               @ius     
php71u-gd.x86_64                    7.1.12-1.ius.centos7               @ius     
php71u-intl.x86_64                  7.1.12-1.ius.centos7               @ius     
php71u-json.x86_64                  7.1.12-1.ius.centos7               @ius     
php71u-mbstring.x86_64              7.1.12-1.ius.centos7               @ius     
php71u-mcrypt.x86_64                7.1.12-1.ius.centos7               @ius     
php71u-mysqlnd.x86_64               7.1.12-1.ius.centos7               @ius     
php71u-opcache.x86_64               7.1.12-1.ius.centos7               @ius     
php71u-pdo.x86_64                   7.1.12-1.ius.centos7               @ius     
php71u-soap.x86_64                  7.1.12-1.ius.centos7               @ius     
php71u-xml.x86_64                   7.1.12-1.ius.centos7               @ius  

```
### 安装php71u-imap

```
yum insatll php71u-imap -y
```

### 修改配置
```
cp /usr/lib64/php/modules/imap.so  /usr/local/php/lib/php/extensions/no-debug-non-zts-20160303/imap.so

vim path/php.ini

添加  extension = "imap.so"


```
### 重启php-fpm

### 不存在php71u-soap 的时候。
> 请到这里下载[https://pkgs.org/download/php70u-imap](https://pkgs.org/download/php70u-imap)








