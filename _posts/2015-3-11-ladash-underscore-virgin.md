---
layout: post
category : php
title:  "Lodash 与 Underscore 浅尝辄止"
date:   2015-03-04 16:15:50
tagline: "Virgin"
tags : [virgin, lodash, underscore, js, jquery]
---
## Lodash 与 Underscore 浅尝辄止
### 链接
- `Lodash` [http://lodash.com](https://lodash.com/docs/)
- `Underscore`
	- 中文 [http://www.css88.com/doc/underscore/](http://www.css88.com/doc/underscore/)
	- 官网 [http://underscorejs.org/](http://underscorejs.org/)
	
### 介绍
> lodash 与 underscore 可以理解为js的函数库。它将弥补jquery在数据操纵函数上的不足。
他们有一百多个函数，用法相似，对象名为`_` 

### 实例
```
是否包含给定键
_.has({a:1,b:2,c:3},"b");
=> true
```

```
以白名单过滤对象
_.pick({name: 'moe', age: 50, userid: 'moe1'}, 'name', 'age');
=> {name: 'moe', age: 50}
_.pick({name: 'moe', age: 50, userid: 'moe1'}, function(value, key, object) {
  return _.isNumber(value);
});
=> {age: 50}
```