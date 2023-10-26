# 26 Redux & RTK Query

Redux是一个稳定且安全的**状态管理器**，作用类似于 state 和 context。只要是 js 应用，都可以用 redux 去管理。
### 1. State 与容器（container）
  1. 当 state 变化时，界面也会随之改变
  2. 容器：存储 state，管理 state，是 state 的管理器。类似于<font color='red'>封装</font>
    1. 所有的修改都在容器中进行
### 2. 特点
  1. 适用于大型项目，功能比 reducer 和 context 更强大
  2. 可预测
    1. 只能进行 action 中的操作（未定义的操作就无法进行，不会出现突发情况）
### 3. 在网页中使用 redux 的步骤
  1. 引入 redux 的核心包
  2. 创建 reducer 整合函数
  3. 通过 reducer 对象创建 **store（容器）**：存储所有 state
  4. 对 store 中的 state 进行订阅 --> 当 store 中的 state 发生变化，可以自动去执行某代码，使得变化自动执行
  5. 通过 dispatch 派发 state 的操作指令
### 4. 一些初步问题
  1. state 过于复杂的话，维护比较麻烦
    可以对 state 进行分组，创建多个 reducer，然后将其合并为一个
  2. state 每次操作时，都需要对 state 进行复制，然后才能修改
  3. case 后面的常量维护起来会比较麻烦
### 5. Redux toolkit( RTK) 
  - `react-redux`
  - `@reduxjs/toolkit`

##### 5.1 slice & store & reducer

  通过根 reducer 将所有 `slice` 的 reducer 组合在一起，使你能够在整个应用程序中访问和操作这些数据。

##### 5.2 完成 RTK 代码

  - 整个 redux 里只有一个 store，放在 index.js 中，用`<Provider></Provider>`包起来，这样包起来的组件（一般是`<App/>`）就能访问到所有数据了
  - 读取/加载 state 中的数据：`useSelector(state => state)`

#####5.3 state 的更新

state 由 reducer 更新，它存放在 store 中
  1. 首先在 reducer 中定义初始 state：`initialState`，一般是一个对象（RTK 中直接指定初始对象）
  2. 执行某个操作，就会调用 dispatch 派发一个 action
  3. store 接收到 action 后，将其传递给与之相关联的 reducer 函数
  4. Reducer 函数会检查 action 的 type，然后根据需要对状态进行更新
  5. Reducer 返回的新状态会替代旧状态，从而更新了应用程序的整体状态
  6. 组件`connect()`到redux store来订阅状态的变化

#####5.4 RTK 函数解读

  1. `createSlice()`，创建 reducer 和 action creators，自动生成初始状态。这个初始状态可在函数外先指定好，然后再在函数中指定
### 6. RTK Query
RTK解决state的问题，RTK Query处理数据加载的问题，主要是发送请求

##### 6.1 构成

  1. 主要对象：API
  2. 类似于RTK的slice，RTKQ是创建不同的**API slice**
  3. 直接用createApi()，就会生成一个API对象，内部自动生成钩子，RTKQ的所有功能都需要通过该对象来进行
    1. 参数：对象


#####6.2 创建API对象

- reducerPath
- baseQuery
- endpoints：回调函数

````js
const studentApi = createApi({
	reducerPath: 'studentApi',  // API标识，可以和前面的变量名一致，但不能和其他API或reducer重复
	baseQuery: fetchBaseQuery({baseUrl: '要发送请求的地址'}),  // 指定查询的基础信息，发送请求使用的工具
  endpoints(build) {
    // build是请求的构建器，通过build来设置请求的相关信息
    return {
      // 操作的名字：操作的类别
      getStudents: build.query({
        // query()用来指定请求的子路径
        // 基础路径 + 子路径 = 完整请求路径
        query(){return 'students';}
      }),
    };
  }  // 用来指定api中的各种功能，是一个方法，需要一个对象作为返回值
});
````

<img src="/Users/liuqiwei/Library/Application Support/typora-user-images/image-20231022161730496.png" alt="image-20231022161730496" style="zoom:60%;" />

##### 6.3 怎么使用？

API对象创建后，对象中会根据各种方法自动生成对应的钩子函数，通过这些钩子函数可以向服务器发送请求。

- **钩子函数命名规则**：getStudents --> useGetStudentsQuery

- **原理**

  - 数据 --> data

  - 数据是否加载成功 --> isSuccess

  - 数据是否正在加载 --> isLoading

    <img src="/Users/liuqiwei/Library/Application Support/typora-user-images/image-20231026145435340.png" alt="image-20231026145435340" style="zoom:65%;" />

##### 6.4 整体过程

1. 创建API对象
   - reducerPath：API对象的名称（id）
   - baseQuery：查询的地址
   - endpoints：查询的具体功能
2. 创建好API对象后，对其进行解构（钩子函数），向外部导出；同时导出API对象
3. 配置store： `configureStore()`
   - reducer：使用API对象名称
   - middleware：使中间件具有缓存功能
4. 导出store
5. store“注入”provider
6. 使用时要导入对应的钩子函数
