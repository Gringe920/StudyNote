# async与await

## async 

### 用法
作为一个关键字，放到一个函数的前面，来表示这个函数是个异步函数。也就是说不会阻塞后面代码的执行。

```
	async function abc(){
		return 'hello async';
	}	
```
返回了一个promise对象，会自动调用`promise.resolve()`包装，无非就是用return值包装了一下。所以如果要获取返回值需要用then方法。

```
//调用
abc().then(result => {
	console.log(result);
})

```

## await

### 用法
等待后面的结果，看跟着是个表达式或者函数。
作为一个关键字，后面可以放任何表达式。注意await关键字只能放到async函数里面。搭配使用。

```
function abc(num){
	return new Promise((resolve, reject) => {
		setTimeout(() => {
			resolve(2 * num);
		}, 2000)
	})
}
async function test(){
	let r = await abc(1);
	console.log(r);
}

test(); //控制台2s之后输出2

// 当有多个请求需要同时进行时，可以用await多个调用之后再执行相应代码。  
```

## 一道面试题目

```
async function async1(){
    console.log('----async1 start-----');
    await async2();
    console.log('----async1 end-----');
}

async function async2(){
    console.log('async2');
}

console.log('script start');

setTimeout(function() {
    console.log('setTimeout');
},0);

async1();

new Promise(function(resolve){
    console.log('promise1');
    resolve();
}).then(function(){
    console.log('promise2');
})

console.log('script end');

//浏览器输出
script start
----async1 start-----
async2
promise1
script end
promise2
----async1 end-----
setTimeout

```





