
* 针对移动浏览器端开发页面，不期望用户放大屏幕，且要求“视口（viewport）”宽度等于屏幕宽度，视口高度等于设备高度，如何设置？

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no"/>
```

* `data-*` 属性的作用是什么？

	* 目前用到的最多是，对添加`data-*`的`DOM`元素进行点击绑定监听，需要用在逻辑处理的参数，用`data-*`传参。比如跳转链接需要带的参数，弹窗展示等。

	* 还有一种，通过`data-*`操作`css`样式，用`js`找到添加`data-*`的`DOM`元素并添加所需属性的`css`，在`css`上写入所属样式，但是这种操作个人觉得并不灵活。

```JavaScript
<div id="content" data-user-list="user_list">data-user_list自定义属性 </div>

//js
var content= document.getElementById('content');
alert(content.dataset.userList)

//jQuery
$('#content').data('userList');//读
$('#content').attr('data-user-list');//读
```