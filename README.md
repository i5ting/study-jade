study-jade
==========

Jade是一款高性能简洁易懂的模板引擎，Jade是Haml的Javascript实现，在服务端（NodeJS）及客户端均有支持。

官网 http://jade-lang.com/


习惯jade的最好办法：找一个已写好的html代码，用jade重写一遍
happy.jpg

但是如果你是新手，而且直接拿jade写没有写过的页面，那么你会死的很难看

## 规则说明

### 标签简写

	比如`<p>`写成`p`
	
jade里的

	p
	
等于

	<p></p>

### 属性放到括号里

	比如`<div class='left_panel'>`写成`div`

#### 标签只有1个属性的情况

jade里的
	
	div(class='left_panel')
	
等于

	<div class='left_panel'>
	
#### 标签如果有多个属性的情况


jade里的

	div(id='this_is_div_id',class='left_panel')
	
等于

	<div id='this_is_div_id' class='left_panel'>
	
	
### value写法

比如html代码
	
	<p>this is a p tag</p>
	
在jade里需要成

	p this is a p tag
	
一定要注意`p`标签后面的空格，如果没有空格是错的

但是如果标签有括号属性的，可以没有空格

	button(id='create_exam_on_server_btn')生成问卷

建议：不管有没有标签，都要再写value的时候加一个空格

### 没有空行

jade里不允许有空行，比如我们在html里可以写的很随意

	<p>前端专业八级考试</p>
	
	<p>友情提示：不准携带通讯工具，不准交头接耳、 一经发现，取消考试成绩，并终生禁止再次参与本考试！一定要记得哦！</p>

但是在jade里，是不允许出现空行，你只能这样写

	p 前端专业八级考试
	p 友情提示：不准携带通讯工具，不准交头接耳、 一经发现，取消考试成绩，并终生禁止再次参与本考试！一定要记得哦！

好处是代码不能太随意，因为编译不通过，jade其实是把空行当做编译一个标签的条件，所以你只能遵守规则玩


### 层级嵌套

比如我们常见的html代码

	<ul id='all_qs'>
		<li>1</li>
		<li>2</li>
		<li>3</li>
	</ul>
	
在jade里需要成

	ul(id='all_qs')
		li 1
		li 2
		li 3

这个实现原理很简单，就是利用缩进来判断包含关系。

缩进有2种方式

1. 空格
2. tab（推荐使用tab）

### 变量

```
doctype html
html
  head
    title= title
    link(rel='stylesheet', href='/stylesheets/style.css')
    script(src='/javascripts/jquery.min.js')
  body
    block content
```

这里的`title= title`代码里的`=`代表后面接的是变量。而且在子页面通过extends继承的也可以使用该变量。


### 内插法

	p #user #{name} #{email}
	
	
### 反转义变量!{html}

	var html = "<script></script>"

	!{html}
	
### 注释

#### 块注释

	body
		//
		#content
			h1 CSSer,关注Web前端技术

#### 条件注释

	body
	/if IE
	    a(href='http://www.mozilla.com/en-US/firefox/') Get Firefox

### 块扩展

块扩展允许创建单行的简洁嵌套标记，下面的示例与上例输出相同：

  ul
    li.first: a(href='#') foo
    li: a(href='#') bar
    li.last: a(href='#') baz
		
		
		
## 布局

layout.jade

```
doctype html
html
	head
		title= title
		link(rel='stylesheet', href='/stylesheets/survey.css')
		script(src='/javascripts/jquery.min.js')
		script(src='/javascripts/jquery.min.js')
		script(src='/javascripts/survey.js')
	body
		div(class='left')
			block left_content
		div(class='main')
			block main_content
```		

集成

```
extends ../layout

block left_content
	h1= title
	button(id='create_exam_on_server_btn')生成问卷
	hr 
	p all 题目
	ul(id='all_qs')

block main_content
	h1= title
	div(class='create_exam')
		select(name='is_ad')
			option(value='true') true
			option(value='false',selected) false
		br
		input(type='text' name='all_name' value='前端技术专业八级考试')
		br
		input(type='text' name='all_desc' value='友情提示：不准携带通讯工具，不准交头接耳、 一经发现，取消考试成绩，并终生禁止再次参与本考试！一定要记得哦！')
		br
		input(type='text' name='all_weixin_name' value='all_weixin_name')
		br
		input(type='text' name='all_weixin_id' value='188888888')
		button(id='create_exam_btn')创建试卷
	div(class='show_exam')
		p 前端专业八级考试
		p 友情提示：不准携带通讯工具，不准交头接耳、 一经发现，取消考试成绩，并终生禁止再次参与本考试！一定要记得哦！
```

说明

- extends是指当前jade页面继承自哪个layout
- block是指定义此处有模板块

## include

## 最佳实践


jade效率基准测试&预编译&nodejs工作流

我想出的满足当下场景下需求(在使用了预编译的同时实现实时刷新)的办法是: nodejs中fs.watch监控jade文件进行re-compile + gulp中watch文件然后延时触发lr.changed().

### 预编译

### livereload


### render

## 总结

- 使用tab而非space
- 不管有没有属性，标签和value之间都要有空格
- 利用layout定义布局模板
- 利用include来引用公共模块
- 预编译，提高模板执行效率
- livereload，提高开发效率


- http://paularmstrong.github.io/node-templates/
- Jade模板引擎入门教程http://www.cnblogs.com/fullhouse/archive/2011/07/18/2109945.html