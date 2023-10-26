# 21 & 22 Strapi & Fetch API

Fetch是 JS 本身就有的一种方式，用于在浏览器中进行网络请求和与远程服务器通信。它提供了一组方法，用于发起 HTTP 请求，并处理请求和响应。

本节的内容是将之前项目中写死的数据替换为从接口 localhost:1337 中加载的数据。组件初始化时需要向服务器发送请求来加载数据。

### 1. Fetch 是什么？

`fetch()`是 Ajax 的升级版，用来向服务器发送请求加载数据

1. 参数：请求地址，请求信息（可省略）

2. 返回一个 promise

   1. `.then(() => {})`：回调函数在请求发送成功时调用

   1. `.catch(() => {})`：回调函数在请求出错时调用

1. ```JavaScript
   useEffect(() => {
     // 在effect中加载数据
     // fetch()是 Ajax 的升级版，用来向服务器发送请求加载数据
     fetch('http://localhost:1337/api/students')
     
     	// response表示响应信息
   		.then((Response) => {
       
       // 将响应的json数据转化为js对象
       return response.json();
   		})
     
     
   		// data是数据库中返回的数据
   		.then((data) => {
       	console.log(data.data);
       	// 将加载到的数据设置到StuData的state设置函数中
       	setStuData(data.data);
   		})
       .catch(() => {
       })
   })
   
   
   
   ```

### 2. 请求方法名称

1. 创建：post
2. 删除：delete
3. 更新：put
4. 查询：get，没有body

### 3. 数据加载提示

当数据加载的网络速度比较慢的时候（比如数据量庞大、网速不好时），需要有一个“正在加载数据”的提示。

方法：通过一个**新的state**来设置

```javascript
const [loading, setLoading] = useState(false);

useEffect(() => {
  setLoading(true);
  ...
}

return (
  <div className="app">
  	{!loading && <StudentList stus={stuData}/>}
    {loading && <p>数据正在加载中...</p>}
  </div>
)
```

### 4. 处理出错的情况

例如：权限不够、路径错误时，如何处理这些错误？

