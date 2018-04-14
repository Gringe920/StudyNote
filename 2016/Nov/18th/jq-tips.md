遍历class元素
```javaScript
	$(element).each(function(i,item){});
```

this的使用
```js
	let test = function(){
		$(element).each(function(i,item){
			$(item).on('change',function(){
				let _this = this;
				fn(_this);//正确
				fn($(this)); //错误
			});
		});
	}
	
	let fn = function(_this){
		$(_this).css() //这样方可拿到传递过来的元素
		_this.css() //错误示例
	}
```