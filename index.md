---
layout: page
title: 知识归纳!
tagline: 一个一天
---
{% include JB/setup %}

## 博文来源 与 计划
- 自制 Sublime 片段
- 工作任务的Markdown
- 收藏转载

## 博文列表

<ul class="posts">
	{% for post in site.posts %}
		<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}
</ul>

## To-Do
### String
### Array
### Json
### Regular
### file
### image
### url
### Session and Cookie
### MySql
### Cache


In `_config.yml` remember to specify your own data:
		
		title : My Blog =)
		
		author :
			name : Name Lastname
			email : blah@email.test
			github : username
			twitter : username

The theme should reference these variables whenever needed.
		
## Sample Posts

This blog contains sample posts which help stage pages and blog data.
When you don't need the samples anymore just delete the `_posts/core-samples` folder.

		$ rm -rf _posts/core-samples

Here's a sample "posts list".

<ul class="posts">
	{% for post in site.posts %}
		<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}
</ul>

## To-Do

This theme is still unfinished. If you'd like to be added as a contributor, [please fork](http://github.com/plusjade/jekyll-bootstrap)!
We need to clean up the themes, make theme usage guides with theme-specific markup examples.


