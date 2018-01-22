---
title: Magento can not delete attribute options
date: 2018-01-22
tags: [magento]
categories:
- 技术
- Magento

---

The attribute has more than 200 options.  Is maybe the high number of the options causing this problem?

The nginx error log below 
```
2018/01/22 14:21:23 [error] 4034#0: *335424 FastCGI sent in stderr: "PHP message: PHP Warning:  Unknown: Input variables exceeded 1000. 
To increase the limit change max_input_vars in php.ini. in Unknown on line 0" while reading response header from upstream

```

<!--more-->

Solution:

### 1.change the php.ini config of `max_input_vars` = 1000 to  `max_input_vars` = 5000


### 2.restart or reload the php-fpm. then have a try.

