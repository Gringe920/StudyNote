###Q&A

* 一开始使用 `$ npm install webpack -g` 把 Webpack 已经安装到了全局环境下
打`webpack`命令
但出现了以下错误

```
PS F:\Daily-note\webpack\first-webpack> webpack
module.js:442
    throw err;
    ^

Error: Cannot find module 'webpack/lib/node/NodeTemplatePlugin'
    at Function.Module._resolveFilename (module.js:440:15)
    at Function.Module._load (module.js:388:25)
    at Module.require (module.js:468:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (F:\Daily-note\webpack\first-webpack\node_modules\html-webpack-plugin\lib\com
    at Module._compile (module.js:541:32)
    at Object.Module._extensions..js (module.js:550:10)
    at Module.load (module.js:458:32)
    at tryModuleLoad (module.js:417:12)
    at Function.Module._load (module.js:409:3)
    at Module.require (module.js:468:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (F:\Daily-note\webpack\first-webpack\node_modules\html-webpack-plugin\index.j
    at Module._compile (module.js:541:32)
    at Object.Module._extensions..js (module.js:550:10)
    at Module.load (module.js:458:32)
```

原因是因为Webpack没有安装到项目的依赖中
使用`$ npm install webpack --save-dev`就搞定了


```
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
```


* 使用`npm install sass-loader --save-dev`，增加scss文件，`npm start`但是出现了一下错误

```
ERROR in Cannot find module 'node-sass'
 @ ./app/main.scss 4:14-115 13:2-17:4 14:20-121
```

原因是缺少了'node-sass'，使用`npm i node-sass —save-dev`就搞定了










