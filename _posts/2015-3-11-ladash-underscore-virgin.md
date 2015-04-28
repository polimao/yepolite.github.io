---
layout: post
category : php
title:  "Lodash 与 Underscore 浅尝辄止"
date:   2015-03-04 16:15:50
tagline: "Virgin"
tags : [virgin, lodash, underscore, js, jquery]
---
### 链接

* `Lodash` [http://lodash.com](https://lodash.com/docs/)
* `Underscore`
	* 中文 [http://www.css88.com/doc/underscore/](http://www.css88.com/doc/underscore/)
	* 官网 [http://underscorejs.org/](http://underscorejs.org/)
	
### 介绍

> lodash 与 underscore 可以理解为js的函数库。它将弥补jquery在数据操纵函数上的不足。
他们有一百多个函数，用法相似，对象名为`_` 

### 实例

	是否包含给定键
	_.has({a:1,b:2,c:3},"b");
	=> true

---

	以白名单过滤对象
	_.pick({name: 'moe', age: 50, userid: 'moe1'}, 'name', 'age');
	=> {name: 'moe', age: 50}
	_.pick({name: 'moe', age: 50, userid: 'moe1'}, function(value, key, object) {
	  return _.isNumber(value);
	});
	=> {age: 50}

## Omnipay 多网关支付处理库
http://omnipay.thephpleague.com/gateways/third-party/

## 如何上传公钥 获得私钥
https://cshall.alipay.com/support/help_detail.htm?help_id=477353&keyword=%CB%BD%D4%BF&sToken=s-8f955fc9a40146ef8291d82aa2985ac1&from=search&flag=2