#ES6基础

`export default xxx`
`import xxx from '..'; xxx()`

## 常量（只读，不可改）

### ES5常量的写法

```ES5
Object.defineProperty(window, 'PI2', {
	value: '3.141592',
	writable: false
})

console.log(window.PI2)
```

### ES6常量的写法

```
const PI2 = '3.1415926'
console.log(PI2);
```

## 作用域

### ES5中的作用域

```js
const callbacks = [];
for (var i = 0; i <= 2; i++){
	callbacks[i] = function(){
		return i * 2
	}
}

console.log(callbacks[0]); //4
console.log(callbacks[1]); //4
console.log(callbacks[2]); //4
```
存在着变量提升

### ES6中的作用域

```
const callbacks = [];
for (let i = 0; i <= 2; i++){
	callbacks[i] = function(){
		return i * 2
	}
}

console.log(callbacks[0]); //0
console.log(callbacks[1]); //2
console.log(callbacks[2]); //4
```
块作用域，这时候的闭包取决于当前块作用域，会把当前块作用域的那个值保存下来供后面的闭包使用，每循环一次，就形成一次新的作用域，那么闭包就导向闭包作用域的变量。

ES6 `{ }`代表一个作用域，ES5以下的需要用立即执行函数进行隔离。

### 箭头函数

改变this指向，指向函数作用域块外的函数本身。

### 默认参数

```
function (x, y = 7, z = 8){
	return x + y + z;
}

```

#### 不确定参数的个数，求全部参数的总和。

``` 
//ES3、ES5 借助arguments
{
	function f(){
		var a = Array.prototype.slice.call(arguments); //获取到当前传入函数的变量数组
		var sum = 0;
		a.forEach(function(item) {
			sum += item * 1;
		})
		return sum
	}
	console.log(f(1,2,3,4));
}
```

```ES6
{
	//ES6 ... 表示扩展运算符 a表示可变参数的列表，是个数组
	function f (...a){
		var sum = 0;
		a.forEach(item => {
			sum += item*1;
		})
		return sum;
	}
	console.log(f(1, 2, 3, 4))
}
```

### 扩展运算符的用法

#### 合并数组
```
{
	//ES5合并数组
	var params = ['hello', true, 9];
	var other = [1, 2].concat(params);
	console.log(other); // [1,2,'hello', true, 9]
}
```

```
{
	//ES6 使用扩展运算符合并数组
	var params = ['hello', true, 9];
	var other = [1, 2, ...params];
	console.log(other);
}
```

### 对象代理

声明一个对象，对某些属性只读不可写。

```JS
{
	//ES3
	var Person = function(){
		var data = {
			name: 'es3',
			sex: 'male',
			age: '15'
		}
		this.get = function(key){
			return data[key];
		}
		this.set = function(key,v){
			if(key != 'sex'){
				data[key] = v;
			}
		}
	}
	
	var person = new Person();
	person.get('name');
	person.set('name', 'aaa');
	person.set('sex', 'female'); //无效
}
```

```
{
	//ES5
	var Person = {
		name: 'es5',
		age: '16'
	}
	Object.defineProperty(Person, 'sex', {
		writable: false,
		value: 'female'
	})
	Person.name = 'c-name'; //c-name
	Person.sex = 'male'; //报错
	
}
```

```
{
	//ES6
	let Person = {
		name: 'es6',
		age: '16',
		sex: 'female'
	}
	let person = new Proxy(Person, {
		get(target, key){
			return target[key]
		},
		set(target, key, value){
			if(key != 'sex'){
				target[key] = value;
			}
		}
	})
}
```

