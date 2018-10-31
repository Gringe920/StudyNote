Redux数据流

页面渲染完之后会有UI，当用户开始触发一些操作的时候会产生Action，这些Action会发送给Reducers里，Reducers会更新Store信息，存在Store里的State便会重新渲染UI。

一切事物皆对象，对象是什么，只不过是属性跟方法的集合。  

- Action

  其实就只是个对象，一般由方法生成，必须要有type，action是作用于store上

- Reducer

  action产生的响应，会发送给reducer，是对响应的抽象，也是由方法生成的，生成一个匿名函数。
传入当前的state，传入作用的action，再根据action的type来决定怎么做，要返回一个新状态。
  reducer根据Store响应

- Store

  所有Reducer都放在Store里，其实就是State跟Reducer的总和。所有数据与状态的存储。 Store是唯一的。state是在运行的时候才产生的，所以看到的时候只存在定义好的reducer。利用Redux库里面的CreateStore(Reducer)，把reducer传进去，便返回Store。




 