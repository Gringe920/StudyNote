### 反转字符串

- 第一种方式

  ```js
  function reserveString(str) {
  	return str.split('').reserve().join('') //split切割成数组，reserve进行数组反转，join拼接成字符串
  }

  ```

  ​

- 第二种方式

  ```js
  function reserveString(str){
     var newStr = '';
     for(var x = str.length - 1; x > 0; x --){
         newStr += str.charAt(x); //从最后一个下标开始读取，拼接到一个新字符串里，charAt返回字符串当前下标那个值，例如abc, index=2, c
     }
     return newStr;
  }
  ```



### 爬楼梯算法题

给定一个楼梯值，解出有多少种爬的方式，每次只有1步或者2步。

例如，传入2，有 （1 ，1）、（2） 两种方式

​	   传入3，有（1,1,1）、（1,2）、（2,1）是那种方式

其实这种思想很像一张树状图

​			3

​		 /              \

​	      1		   2

​	/	      \			\

​     1			2		   1

 /

1

```js
//暂时只想到递归的方法，比较耗时，不推荐
function climbStep(n){
    if(n == 0 || n == 1 || n == 2 || n - 3){ //特殊情况先排除，例如一层只有一种方式爬，两层有两种方式...
        return n;
    }
    return climbStep(n - 1) + climbStep(n - 2) //接下来可能出现走一步，或者走两步的情况
}
```

