## webpack知识点总结

### webpack的作用
webpack的作用是：模块化js打包、es6/ts等语言的重新编译、css预处理器编译，转化为合适的格式供浏览器使用。

### webpack的基本使用
#### 最初始在终端使用
```bash
#  <enrty file> 填入入口文件的路径
#  <destination for bundled file> 填写打包文件的存放路径
#  填写的时候无须加上 <>
webpack <entry file> <destination for bundled file>
```

#### 在配置文件中使用 webpack.config.js
```javascript
module.exports = {
    entry: __dirname + '/app/main.js',
    ouput: {
        path: __dirname + '/public',
        filename: 'bundle.js'
    }
}
```
注：`__dirname`是指当前脚本所在的目录，是个node.js中的一个全局变量
在终端执行`webpack`会自动引用文件中的配置选项。

#### npm可以引导任务执行，可以替代略微繁琐的命令 package.json
加入字段

```json
"script": {
	"start": "webpack"
}
```

注：npm 的 start 命令较为特殊，在命令行中使用npm start就可以执行起对应的命令。
如果命称不是start，就得使用`npm run <script name>`。

#### webpack核心功能了解一下
##### sourceMap 
在浏览器打开开发者模式的时候，我们看到的是一份打包后的js文件，可以通过配置`sourceMap`追溯到自己想要的那个源文件，即在`webpack://`文件夹下可看到。
在webpack.config.js中配置devtool参数

```
module.exports = {
    entry: __dirname + '/app/main.js',
    ouput: {
        path: __dirname + '/public',
        filename: 'bundle.js'
    },
    devtool: 'source-map'
}
```

配置参数分别有`source-map`、`cheap-module-source-map`、`eval-source-map`、`cheap-module-eval-source-map`，其分别影响的各有千秋，一般在判断本地or测试环境需要调试的时候配置加上这个参数便可。

##### 本地服务器实时监听
实时监听本地文件变化，可以通过配置`webpack-dev-sever`
只需要三个步骤
第一步，`npm i webpack-dev-server --save-dev`，可以指定自己所需要的某个版本，例如`npm i webpack-dev-server@2.9.7 --save-dev`
第二步，在webpack.config.js配置devServer参数

```
module.exports = {
    entry: __dirname + '/app/main.js',
    ouput: {
        path: __dirname + '/public',
        filename: 'bundle.js'
    },
    devtool: 'source-map',
    devServer: {
        contentBase: './public',  //需要加载本地服务器所在的目录
        inline: true, //修改本地的时候实时刷新
        historyApiFallback: true //不跳转
    }
}
//具体配置可看官方文档
```

第三步，在package.json配置打包命令在`script`参数增加配置，然后点击`npm run dev-server`，打开localhost:8080便可看到你的文件

```
"script": {
	"dev-server": "webpack-dev-server"
}
```

##### 各种各样的loader
loader是啥呢？
> loader 用于加载某些资源文件。 因为webpack 本身只能打包commonjs规范的js文件，对于其他资源例如 css，图片，或者其他的语法集，比如 jsx， coffee，是没有办法加载的。 这就需要对应的loader将资源转化，加载进来。从字面意思也能看出，loader是用于加载的，它作用于一个个文件上。

##### `babel-loader`
##### `css-loader`、`style-loader`
##### `sass-loader`、`less-loader`、`postcss-loader`

#### webpack plugin了解一下
webpack的插件是啥呢？
> plugin 用于扩展webpack的功能。它直接作用于 webpack，扩展了它的功能。当然loader也时变相的扩展了 webpack ，但是它只专注于转化文件（transform）这一个领域。而plugin的功能更加的丰富，而不仅局限于资源的加载。

使用步骤
第一步： 先使用npm下载
第二步： 配置webpack.config.js

```
plugins: [
   new webpack.BannerPlugin('Gringe test') //随便找个plugin为例子，其它插件配置请参照相关文档
]
```

#### 开发模式、生产环境以及正式环境
##### 第一种方式
维护两个配置文件，默认是用`webpack.config.js`，webpack会自动读取这个文件，同时可以增加多一种配置文件 `webpack.config.dev.js` 当然名字随意即可，但可别太随意。这个时候你只要简单地配置一下 `package.json` 的script参数：

```
"scripts": {
  // 运行npm run build 来编译生成生产模式下的bundles
  "build": "webpack --config webpack.config.dev.js",
  // 运行npm run dev来生成开发模式下的bundles以及启动本地server
  "dev": "webpack-dev-server --config webpack.config.dev.js"
 }
```

##### 第二种方式
在`webpack.config.js`配置文件中区分开来，在 `package.json` 使用变量来区分。

由于mac和win在`package.json`设置变量的方式不同，一个用Set，一个用export。

`npm i cross-env` 可以接触这种烦恼。

然后你就可以在package.json狠狠地配置你的变量，这里变量名叫做`NODE_ENV`

```
"scripts": {
  // 运行npm run build 来编译生成生产模式下的bundles
  "build": "cross-env NODE_ENV=production webpack --config webpack.config.dev.js",
  // 运行npm run dev来生成开发模式下的bundles以及启动本地server
  "dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.dev.js"
 }
```

然后在webpack.config.js里面就可以判断

```
const isDev = process.env.NODE_ENV === 'development'
const config = {
    entry: __dirname + '/app/main.js',
    ouput: {
        path: __dirname + '/public',
        filename: 'bundle.js'
    },
    devtool: 'source-map'
}

if(isDev){
	config.devServer = {
		contentBase: './public',  //需要加载本地服务器所在的目录
       inline: true, //修改本地的时候实时刷新
       historyApiFallback: true //不跳转
	}
}

module.exports = config;
```




#### 遇到问题
##### webpack版本降级问题
一开始没留意到 webpack 4 可以移除某些配置项，而且需要固定某些文件夹名称例如`src`，因此提示我这边找不到`./src`等文件夹的错，于是我需要给 webpack 降级。

```linux
npm uninstall -g webpack //全局删除webpack
npm list webpack -g //查看全局webpack是否存在
npm cache --force clean //清楚npm缓存
npm install -g webpack@3.5.0 //添加一个低于版本4的webpack
```

一开始生疏千万别打错某些单词，例如`module.exports`别写成`module.export`，path 注意是文件分割符，比如`/public`别忘记反斜杠`public`;

##### webpack使用es6语法 
将配置文件更改为`webpack.config.babel.js`即可，一开始还是不行，报`SyntaxError: Unexpected token export`，还是不能使用es6语法。
原因因为没有在`.babelrc`里面加上es2015

```
{
  "presets": ["react","es2015"]
}
```

emmmmm...你有可能继续报错，`Couldn't find preset "es2015" relative to directory`，这时候再狠狠地`npm install babel-preset-es2015 --save-dev`便可

ps：这种方式有利有弊，我是看有先人用这种方式配置便使用。网上说法还有其它方式。泥萌可以尝试[点击文章](https://segmentfault.com/a/1190000003932889)自己品尝。

##### hash和chunkhash的区别
chunk是指例如我们在entry声明不同key就是不同的chunk或者使用异步加载的模块，每一个异步加载的模块也是一个chunk。

我们如果使用hash，打包出来的文件都是同个hash，是整个应用的hash
而如果使用chunkhash，他们每个chunk都会生成一个hash，所以他们的hash会有区别







