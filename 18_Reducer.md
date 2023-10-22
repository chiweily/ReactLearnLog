# 18 Reducer

### 1. 定义 & 构成

1. 简单来说就是下命令的，类似于“主管”（管理函数的函数）。用于管理组件内部的状态，将分散在各处的state方法整合在一处，使得代码看起来更规整：**整合函数**
   1. 对于当前state的所有操作都应该在reducer函数中定义
   2. reducer函数的返回值会成为state的新值-->所以直接return就行
2. 参数构成：

`useReducer(函数，useState初始值）`

- reducer函数（state, action）
  - 根据不同的`action.type`采取不同的state操作返回
- useState初始值：一般是`useState()`的初始值

3. reducer在执行时会收到两个参数：

- ![img](https://yqninqydmw.feishu.cn/space/api/box/stream/download/asynccode/?code=MjBlZDUxM2ZiYjA5MmRkNTI1MDhmMjQ2ZTFhMDVkOTZfcVhLbUp1a0Z0WFIxWHQ5cXVTajZzMjF3dGt2Zk96WDlfVG9rZW46WlhQbWJYWE5ub01OU1Z4ZmF3cWMzNE9ibktoXzE2OTc5ODQyOTk6MTY5Nzk4Nzg5OV9WNA)

- state：当前函数中最新的state值
- action.type：一般是一个对象

4. 返回值：是一个数组

- 第一个参数：state，用来获取state的值
- 第二个参数：state修改的<u>**派发器（dispatch）**</u>
  - 原来返回的是setState函数，但现在返回的是state的派发器
  - 通过派发器可以发送操作state的命令，具体的修改行为由另外一个函数执行-->`useReducer(() => {}, ......)`

### 2. 使用

以如下代码为例：

```JavaScript
import React, { useState } from 'react';

const Demo1 = () => {
    const [count, setCount] = useState(1);

    const addHandler = () => {
        setCount(prevState => prevState + 1);
    };

    const subHandler = () => {
        setCount(prevState => prevState - 1);
    };

    return (
        <div>
            <button onClick={subHandler}>减少</button>
            {count}
            <button onClick={addHandler}>增加</button>
        </div>
    );
};

export default Demo1;
```

加入`useReducer()`后：

```JavaScript
import React, { useState, useReducer } from 'react';


// 为了避免reducer会重复创建，定义reducer通常在组件外部进行
// (只会在模块加载时定义一次，其余时间不会再次创建)
const countReducer = (state, action) => {
    switch(action.type) {
        case 'Add':
            return state + 1;
        case 'Sub':
            return state - 1;
        default:
            return state;
    }
};


const Demo1 = () => {
    
    // 钩子函数必须写在组件里面
    const [count, countDispatch] = useReducer(countReducer, 1);
    
    const addHandler = () => {
        countDispatch({type: 'Add'});
    };

    const subHandler = () => {
        countDispatch({type: 'Sub'});
    };

    return (
        <div>
            <button onClick={subHandler}>减少</button>
            {count}
            <button onClick={addHandler}>增加</button>
        </div>
    );
};

export default Demo1;
```