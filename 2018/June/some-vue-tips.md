##一些vue小笔记

### H5上唤起键盘搜索键

```html
<input class="search-text" v-model="searchText" type="search" @keyup.13="searchSubmit" placeholder="xxx"/>
```

移动端要唤起输入键盘把搜索框的`type`改为`search`即可。
在安卓上可看到，但是在ios不行。

```html
<form action="#" id="search-form">
    <input class="search-text" v-model="searchText" type="search" @keyup.13="searchSubmit" placeholder="xxx"/>
</form>
```
只要统一加上`form`标签即可兼容移动端的安卓以及ios。

搜索，调用事件`keyup.13`即可。

可能这时候还需要重写一下样式。


### 使用`$set()`改变数组

```js
    let arrlist = [1, 2, 3, 4];
    this.$set(arrList, 0, '2');
    console.log(arrList); // [2, 2, 3, 4]
```

### `@click`点击被触发几次，点击穿透

使用`@click.once`

```vue
    <a v-on:click.once="doThis"> </a>
```

### 使用全局过滤器增加图片前缀

main.js

```js
Vue.filter('urlFilter', (value) => {
    return host + value
})
```

调用时

```js
<img :src="picPath | urlFilter" alt="">
```


### error Assigning to rvalue 报错
原因是
``` html
    <el-pagination
        @current-change="handleCurrentChange"
        :current-page.sync="1"
        :page-size="6"
        layout="prev, pager, next, jumper"
        :total="totalCount">
    </el-pagination>
```

`current-page.sync` 的值直接写死，应该定义一个变量声明

```js
data(){
    return {
        currentPage: 1
    }
}
```

```html
<el-pagination
        @current-change="handleCurrentChange"
        :current-page.sync="currentPage"
        :page-size="6"
        layout="prev, pager, next, jumper"
        :total="totalCount">
    </el-pagination>
```

将值写入即可

也有可能是v-model绑定的属性并没有在data属性中定义


### `Error in mounted hook: "TypeError: handlers[i].call is not a function`

命周期钩子函数 是否有声明了未定义方法 或是 只声名了钩子函数。





