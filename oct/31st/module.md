#Notes on [learn-module](https://zhuanlan.zhihu.com/p/22890374)

##模块化的作用

* 可维护性
* 命名空间
* 可复用性

##引入模块的方式
>模块模式一般用来模拟类的概念（因为原生JavaScript并不支持类，虽然最新的ES6里引入了Class不过还不普及）这样我们就能把公有和私有方法还有变量存储在一个对象中——这就和我们在Java或Python里使用类的感觉一样。这样我们就能在公开调用API的同时，仍然在一个闭包范围内封装私有变量和方法。

### 匿名闭包函数方式
```

(function(){
	//在函数的作用域中下面的变量是私有的
	var myarray = [12,13,23,4,6];

	var fun1 = function(){
		var every = myarray.every(function(ele,index,array){
			return (ele >= 10);
		});

		return every ? every.length : 'I fail';
	}

	var fun2 = function(){
		var shifted = myarray.shift();

		return 'you had shift the element' + shifted;
	}

	console.log(fun2());
}());

```


`reduce()` 方法接收一个函数作为累加器（accumulator），数组中的每个值（从左到右）开始合并，最终为一个值。

`filter()` 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组。

`every()` 方法测试数组的所有元素是否都通过了指定函数的测试。

>#####立即执行函数（IIFE)
以关键词function开头的语句总是会被解释成函数声明（JS中不允许没有命名的函数声明），而加上括号后，内部的代码就会被识别为函数表达式


###全局引入方式

```

(function (globalVariable) {

  // 在函数的作用域中下面的变量是私有的
  var privateFunction = function() {
    console.log('Shhhh, this is private!');
  }

  // 通过全局变量设置下列方法的外部访问接口
  // 与此同时这些方法又都在函数内部

  globalVariable.each = function(collection, iterator) {
    if (Array.isArray(collection)) {
      for (var i = 0; i < collection.length; i++) {
        iterator(collection[i], i, collection);
      }
    } else {
      for (var key in collection) {
        iterator(collection[key], key, collection);
      }
    }
  };

  globalVariable.filter = function(collection, test) {
    var filtered = [];
    globalVariable.each(collection, function(item) {
      if (test(item)) {
        filtered.push(item);
      }
    });
    return filtered;
  };

  globalVariable.map = function(collection, iterator) {
    var mapped = [];
    globalUtils.each(collection, function(value, key, collection) {
      mapped.push(iterator(value));
    });
    return mapped;
  };

  globalVariable.reduce = function(collection, iterator, accumulator) {
    var startingValueMissing = accumulator === undefined;

    globalVariable.each(collection, function(item) {
      if(startingValueMissing) {
        accumulator = item;
        startingValueMissing = false;
      } else {
        accumulator = iterator(accumulator, item);
      }
    });

    return accumulator;

  };

 }(globalVariable));

```

###对象接口

```
var test = (function () {

  // 在函数的作用域中下面的变量是私有的
  var myarray = [12,13,23,4,6];

  // 通过接口在外部访问下列方法
  // 与此同时这些方法又都在函数内部

  return {
    fun1: function() {
      var every = myarray.every(function(ele,index,array){
			return (ele >= 10);
		});

		return every ? every.length : 'I fail';
    },

    fun2: function() {
      var shifted = myarray.shift();

		return 'you had shift the element' + shifted;
    }
  }
})();

test.fun1(); //"I fail"
test.fun2(); //"you had shift the element12"

```

主要了解以上的代码格式，照搬了文章里面很多方法。


接下来
我需要好好学习以下相关概念
###CommonJS
`CommonJS` 所定义的每个js模块都是私有的，互不影响。用`require`加载模块

example.js
```
var x = 5;
var addX = function (value) {
  return value + x;
};
module.exports.x = x;
module.exports.addX = addX;
```

```
var example = require('./example.js');

console.log(example.x); // 5
console.log(example.addX(1)); // 6

```
###AMD
(Asynchronous Module Definition)
```
define(id?, dependencies?, factory);
```
>异步模块定义规范（AMD）制定了定义模块的规则，这样模块和模块的依赖可以被异步加载。这和浏览器的异步加载模块的环境刚好适应（浏览器同步加载模块会导致性能、可用性、调试和跨域访问等问题）

```
define(['myModule', 'myOtherModule'], function(myModule, myOtherModule) {
  console.log(myModule.hello());
});
```



