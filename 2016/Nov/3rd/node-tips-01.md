

## node的__dirname以及__filename理解 
[REFERENCE](https://nodejs.org/docs/latest/api/globals.html#globals_dirname)

### __dirname

* `__dirname` 目前正在运行的脚本所在的目录

    栗子：

    在`example.js`(来自`/Users/mjr`)里面写着

    ```js
    console.log(__dirname);
    ```

    `node` 环境下 `node example.js`

    将会打印 `/Users/mjr`


* `__dirname`其实不是全局的 而是每一个module的 是局部的

    栗子：

    a,b模块，模块里面的代码都是`console.log(__dirname);`

    b模块依赖于a模块

    a的路径：`/Users/mjr/app/a.js`

    b的路径：`/Users/mjr/app/node_modules/b/b.js`


    node 环境下 `node b.js`，打印

    ```js
    /Users/mjr/app/node_modules/b
    ```

    node 环境下 `node a.js`，打印

    ```js
    /Users/mjr/app
    ```


## __filename
* `__filename`返回目前正在运行的脚本所在的目录的文件名。

    栗子：

    在example.js(来自/Users/mjr)里面写着

    ```js
    console.log(__filename);
    ```

    node 环境下 

    ```bash
    node example.js
    ```

    将会打印 

    ```
    /Users/mjr/example.js
    ```


