* 多行省略
```css
overflow : hidden;
text-overflow: ellipsis;
display: -webkit-box;
-webkit-line-clamp: 2;
-webkit-box-orient: vertical;
```

* li:last-child不起效果
```css
//源代码
<ul class="">
	<li></li>
	<li></li>
	<li></li>
	<hr />
</ul>
```
搜了css手册
`E:last-child { sRules }`
要使该属性生效，E元素必须是某个元素的子元素，E的父元素最高是body，即E可以是body的子元素