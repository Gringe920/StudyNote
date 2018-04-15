前端时间遇到一个面试题，你用node做过哪些应用？当时脑袋有一丢丢空白，莫名语塞。以前会认为node是一个环境，但是没有基于此更完善地磨轮子，学得零零散散。今天开发个node应用，来巩固更完善的node知识。


### 搭建node环境（自行百度）

### 开发node应用
#### 一个简单的http请求
- touch server.js

```js
var http = require('http');

http.createServer(function(req, resp){
    resp.writeHead(200, {"Content-Type": "text-plain"});
    resp.write('hello node-app');
    resp.end();
}).listen(8080);
```
这是个基于事件驱动的回调。你调用了`http.createServer()`返回了个对象，并且监听(`listen`)了8080端口，成功请求后，返回做了匿名函数做的事。

- 让server.js变成一个模块

```js
var http = require('http');

function start(){
    function onRequest(req, resp){
        resp.writeHead(200, {"Content-Type": "text-plain"});
        resp.write('hello node-app');
        resp.end();
    };
    http.createServer(onRequest).listen(8080);
}


exports.start = start;
```

- touch index.js

```js
var server = require('./server');
server.start();
```

- 修改server.js，获取浏览请求的url

```js
var http = require('http');
var url = require('url');

function start(){
    function onRequest(req, resp){
        var pathname = url.parse(req.url).pathname;
        console.log('request for' + pathname + 'received');
        resp.writeHead(200, {"Content-Type": "text-plain"});
        resp.write('hello node-app');
        resp.end();
    };
    http.createServer(onRequest).listen(8081);
}


exports.start = start;

```
- 认识路由，让不同路由执行不同的业务操作
	- touch router.js

		传入动作`handle`以及pathname（路由名称）
		
		如果传入的`handle`是个方法，便执行，否则log。

		```js
		function route(handle, pathname){
		    console.log('this is a router for request ' + pathname);
		    if(typeof handle[pathname] === 'function'){
		        handle[pathname]()
		    }else{
		        console.log('no request for' + pathname);
		    }
		}
		
		exports.route = route;
		```
	- touch requestHandlers.js

		写入两个方法，不同的方法处理不同的逻辑。

		```js
		function start(){
		    console.log('this is start func logic');
		}
		
		function upload(){
		    console.log('this is upload func logic');
		}
		
		exports.start = start;
		exports.upload = upload;
		```

	- 修改index.js

		定义一个对象handel，把`requestHandlers`对应的方法作为（value）赋值给相应的路由名（key），并且把对象传递给server调用的函数start。

		```js
		var server = require('./server');
		var router = require('./router');
		var requestHandlers = require("./requestHandlers");
		
		var handler = {};
		handler['/'] = requestHandlers.start;
		handler['/start'] = requestHandlers.start;
		handler['/upload'] = requestHandlers.upload;
		
		server.start(router.route, handler);
		```

	- 修改server.js

		接收传递进来的参数handel，传回给router调用。

		```js
		var http = require('http');
		var url = require('url');
		
		function start(route, handle){
		    function onRequest(req, resp){
		        var pathname = url.parse(req.url).pathname;
		        route(handle, pathname);
		        resp.writeHead(200, {"Content-Type": "text-plain"});
		        resp.write('hello node-app');
		        resp.end();
		    };
		    http.createServer(onRequest).listen(8081);
		}
		
		exports.start = start;
		
		```

- 在不同路由上做不同的事情

	- 修改server.js，将resp作为参数传给router

		```js
		var http = require('http');
		var url = require('url');
		
		function start(route, handle){
		    function onRequest(req, resp){
		        var pathname = url.parse(req.url).pathname;
		        route(handle, pathname, resp);
		        // resp.writeHead(200, {"Content-Type": "text-plain"});
		        // resp.write('hello node-app');
		        // resp.end();
		    };
		    http.createServer(onRequest).listen(8081);
		}
		
		
		exports.start = start;
		```
	- 修改router.js，将resp传入给handler
		
		```js
		function route(handle, pathname, reps){
		    console.log('this is a router for request ' + pathname);
		    if(typeof handle[pathname] === 'function'){
		        handle[pathname](reps)
		    }else{
		        console.log('no request for' + pathname);
		    }
		}
		
		exports.route = route;
		```
	- 修改requestHandler.js
		对resp做不同的处理。
		
		```js
		var exec = require("child_process").exec;

		function start(response) {
		  console.log("Request handler 'start' was called.");
		
		  exec("ls -lah", function (error, stdout, stderr) {
		    response.writeHead(200, {"Content-Type": "text/plain"});
		    response.write(stdout);
		    response.end();
		  });
		}
		
		function upload(response) {
		  console.log("Request handler 'upload' was called.");
		  response.writeHead(200, {"Content-Type": "text/plain"});
		  response.write("Hello Upload");
		  response.end();
		}
		
		exports.start = start;
		exports.upload = upload;
		```

不同路由请求展示不同界面。
请欣赏渣渣[demo](https://github.com/Gringe920/sundae/simple-req-res)


- 再此基础上，做个表单发送请求并返回展示
略。




  