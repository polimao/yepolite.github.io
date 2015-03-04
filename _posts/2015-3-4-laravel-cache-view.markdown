---
layout: post
title:  "Serialization of 'Closure' is not allowed"
date:   2015-03-04 16:15:50
---

## 页面缓存报错：Serialization of 'Closure' is not allowed

	现在我要给几个页面做缓存
	
> 首先我想到的是生成静态页，判断静态页是否存在，如果存在就直接访问静态页。但通过对Laravel中的Caching学习以及下面这个博文的了解让我决定使用将整个模板数据缓存的方式。【[laravel使用Caching缓存数据减轻数据库查询压力](http://blog.csdn.net/xd43100678/article/details/24377531)】

```
<?php
public function model()
{
	$ckey = 'Topka_Cars_model_Html';

	if(Cache::has($ckey))
		return Cache::get($ckey);  
	$view = View::make('cars.model');

	Cache::put($ckey, $view, 21600);

	return Cache::get($ckey);  
}

```
### 然而这方法是对的没错，却让我陷入另一个烦恼
	Serialization of 'Closure' is not allowed
苦苦寻找，终于让我在谷歌上寻找到一个解决方案：
http://stackoverflow.com/questions/26175867/laravel-caching-view-errors-serialization-of-closure-is-not-allowed

`下面是解决之道`

```
<?php
public function model()
{
	$ckey = 'Topka_Cars_model_Html';

	if(Cache::has($ckey))
		return Cache::get($ckey);  
	$view = View::make('cars.model')->render();

	Cache::put($ckey, $view, 21600);

	return Cache::get($ckey);  
}

```