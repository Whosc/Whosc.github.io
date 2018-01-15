---
title: Magento1.x paypal 支付不成功问题
date: 2018-01-15
tags: [magento]
categories:
- 技术
- Magento

---

之前magento 订单一直正常，后来查看订单的状态一直是在pengding 状态，查看后台报错了
```
Your current version of cURL php5 module is 7.29.0, which can prevent services that require TLS v1.2 from working correctly. It is recommended to update your cURL php5 module to version 7.34.0 or higher.
```

<!--more-->

问题明显是要我们升级linux curl 的版本


### 查看curl 的版本
```
curl --version
```

### 创建一个源文件,并添加下面的内容:
```
 vim  /etc/yum.repos.d/city-fan.repo
 
[CityFan]
name=City Fan Repo
baseurl=http://www.city-fan.org/ftp/contrib/yum-repo/rhel$releasever/$basearch/
enabled=1
gpgcheck=0
 
```
### 开启源并且更新curl
```
yum install epel-release -y

yum update curl

```

### 检查curl 是否更新成功
```
curl --version
```

### 重启php-fpm 

```
path/php-fpm restart

```

再次登录在magento 后台，上面的报错后消失。测试paypal 支付，成功！




