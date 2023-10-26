# 24 CRUD

### 1. 删除数据

类似添加的过程，method为delete

### 2. 添加数据

1. 通过state去新增数据：设置state --> 设置事件 --> 双向绑定 --> 添加value

2. 创建添加学生的方法：通过`useCallback`添加学生数据，异步，try...catch...finally

   ``````javascript
   const res = fetch('地址', {
     method: 'post', 
     // 请求体
     body: JSON.stringify({data: inputData}),
     // 请求头，告诉服务器数据的格式是json
     headers: {"Content-type": "application/jsn"}
   })
   ``````

3. 添加成功，useContext刷新列表 

### 3. 修改数据

 类似添加的过程，method为put
