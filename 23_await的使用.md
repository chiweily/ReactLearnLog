# 23 await的使用

### 1. 前提介绍

fetch返回的是一个promise。在执行fetch的过程中，读取一次，调用一次回调函数，本质上这个过程是一个异步的过程。await可以用同步的方式去读取异步函数。

### 2. 如何使用await？

要确保await的外部函数是一个异步函数 ==> `async()`

- 使用await之前的方式：then、then、catch

- 使用await之后的方式：useEffect不支持异步，只能先定义一个普通箭头函数，然后在该函数中定义异步函数

  `const fetchData = async() => {}`

​		具体写法如下：

```javascript
useEffect(() => {
  const fetchData = async() => {
    try{
      setLoading(true);
      setError(null);
      const res = await fetch('localhost:3000');
      // 判断请求是否加载成功
      if(res.ok) {
        const data = await res.json();
        setStuData(data.data);
      } else {
        throw new Error('加载失败');
      } catch (e){
        setError(e);
      } finally {
        setLoading(false);
      }
    };
    fetchData();
  }
})
```

​	解释：

​		useEffect函数接受一个回调函数作为参数，这个回调函数会在组件渲染完成后执行。在这个回调函数中，首先定义了一个名为fetchData的异步函数。这个函数用来发送网络请求获取数据。接下来，将loading状态设置为true，表示数据正在加载中，并将error状态设置为null，表示没有错误。

​		然后使用fetch函数发送GET请求到'localhost:3000'，获取数据。如果请求成功（即res.ok为true），则将获取到的数据使用res.json()方法解析为JSON格式，并将解析后的数据通过setStuData函数设置到组件的state中。如果请求失败，则抛出一个错误，错误信息为'加载失败'。

​		在catch块中，将捕获到的错误赋值给error状态，表示数据加载发生错误。最后，不论请求成功还是失败，都将loading状态设置为false，表示数据加载完成。

​		最后，调用fetchData函数来获取数据。由于是在useEffect的回调函数中调用fetchData函数，所以该函数会在组件渲染完成后执行。

