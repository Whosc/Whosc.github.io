---
title: 给laravel 框架的css 、js 设置一个可以手动开关的开发模式
date: 2018-01-23
tags: [php,laravel]
categories:
- 技术
- laravel

---

最近的新项目使用到了laravel 框架开发项目，修改js 的时候传到线上的服务器上，一直刷新没有反应，琢磨着给他设置一个开发模式，可以手动设置开、关。这样在测试的时候方便一些，同时不影响正常的线上网站缓存。

## 定义一个用来设置的常量

```
define("CSSJS_DEBUG" , '0');                        // 开启css js 调试模式，不缓存。 
```
<!--more-->

## 在自定义的自动加载的方法中，添加一个方法.

常量设置成真的时候开启开发模式，不缓存css 和js ,否则没小时自动更新一下缓存。

```
/**
 * css js  调试模式不缓存
 * 
 * @return str
 */
function cssJsDebug() {
	
	if(CSSJS_DEBUG){
		return '?v='.time();
	}else{
		return '?v='. date("Ymdi",time());
	}
}
```


## 给新加的css js 添加后缀，这样可以不缓存css js 的文件。如下：

```

<link href="{{asset('admin/custom/plugins/layui/css/layui.css'.cssJsDebug() )}}" rel="stylesheet">
<script type="text/javascript" src="{{asset('admin/js/jquery-2.1.1.min.js'.cssJsDebug() )}}"></script>

```


