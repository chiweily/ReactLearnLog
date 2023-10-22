# 21 & 22 Strapi & Fetch API

是 JS 本身就有的一种方式，用于在浏览器中进行网络请求和与远程服务器通信。它提供了一组方法，用于发起 HTTP 请求，并处理请求和响应。

本节的内容是将之前项目中写死的数据替换为从接口 localhost:1337 中加载的数据。组件初始化时需要向服务器发送请求来加载数据。

### 1. Fetch 是什么？

`fetch()`是 Ajax 的升级版，用来向服务器发送请求加载数据

1. 参数：请求地址，请求信息（可省略）

2. 返回一个 promise

   1. `.then(() => {})`：回调函数在请求发送成功时调用

   1. `.catch(() => {})`：回调函数在请求出错时调用

1. ```JavaScript
   // response表示响应信息
   .then((response) => {
       // 将响应的json数据转化为js对象
       return response.json();
   })
   // data是数据库中返回的数据
   .then((data) => {
       console.log(data);
   })
   .catch(() => {
   })
   ```

1. 