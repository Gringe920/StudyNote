##请描述一下cookies，sessionStorage和localStorage的区别？

`localStorage`自从工作以来，用过最多的就是`localStorage`，以`key-value`的形式存储在浏览器中，要手动清除浏览器的缓存才能再次拿到想要的`key-value`。嵌入在app里面的页面，安卓则需要删除设置-应用里面的存储数据，ios则需要卸载app，重新安装。实际操作的例子有用户首次进来某个页面，展示所需的业务;手机号码输入，二次输入手动储存等。

不同点:
* 存储大小限制不同。`cookie`数据不超过4k，每次`http`请求都会携带`cookie`，所以值保存很小的数据。其他两个可以达到5M甚至更大。
* 数据有效期的不同。`sessionstorage`尽在当前浏览器窗口关闭前有效，`localstorage`即使关闭浏览器也有效，至于消除，以上有讲。`cookie`呢，在设置过期时间之前都有效的。
* 作用域不同。`sessionstorage`不在不同浏览器窗口中共享，即使是同个页面。其他两个在所有同源窗口中都是共享的。
* 貌似cookie可以在浏览器跟服务器之间来回传递，不过对这个概念还不是很理解。


##解释一下box-model：全部属性，各个属性取值类型，范围，计算值方式，负值作用，box-sizing概念。
box-model include margin,border,padding,content.


`box-sizing`:用于更改用于计算元素宽度和高度的默认CSS框模型。 可以使用此属性来模拟不正确支持CSS框模型规范的浏览器的行为。


##BFC(Block Formatting Context)是什么？有哪些应用？
`Web`页面中盒模型布局的`CSS`渲染模式
应用：如`overflow: scroll, overflow: hidden, display: flex, float: left`,或者 `display: table`来创建。
1. 防止外边距折叠，两个块级元素会有边距折叠，只要它们两个不在同个`BEC`模式里面就ok了。
2. 清除浮动。。哈哈哈用了这么久的属性才发现原来是这个梗。
3. 防止文字围绕
4. 多列布局

##一个元素若宽高已知，如何实现水平、垂直均相对于父元素居中？若这个元素高度未知呢？

```css
.parent{
	position: relative;
}
.children{
	position: absolute;
	left:50%;
	margin-left: -width/2;
	/*以上实现了左右居中*/
}
```
```javascript
var parentHeight = $('.parent').height();
var childrenHeight = $('.children').height();
$('.children').css({
	top:(parentHeight-childrenHeight)/2
});
/*以上实现了上下居中，很麻烦很不好。*/
```

##请优化下段代码：
```javascript
for (var i = 0; i < document.getElementsByTagName('a').length; i++) {
     document.getElementsByTagName('a')[i].onmouseover = function () {
         this.style.color = 'red';
     }
     document.getElementsByTagName('a')[i].onmouseout = function () {
         this.style.color = '';
     }
 }
```

```javascript
var aTags = document.getElementsByTagName('a');
var len = aTags.length;
for (var i = 0; i < len; i++) {
     aTags[i].onmouseover = function () {
     	changeColor(this,'red');
     }
     aTags[i].onmouseout = function () {
        changeColor(this,'');
     }
 }

function changeColor(ele,color){
	ele.style.color = color;
}

```

