# 27 Router5

虚拟路由，不同的路径的访问由客户端路由进行切换（操作在浏览器中通过js进行），不会占用服务器的资源。SPA常用router方法，访问某个页面实际上就是访问该组件（挂载该组件），即：路径（URL）和组件之间进行映射。

当用户访问某个地址时，与其对应的组件会自动挂载。

### 1. 安装

- `npm install react-router-dom@5 -S`

### 2. 使用步骤

1. 引入`react-router-dom`包

2. 在`index.js`中引入`BrowserRouter`组件
3. 将`BrowserRouter`设置为根组件

### 3. 方法（version 5）

将路由和组件进行映射：使用<Route/>，默认情况下Route并不是严格匹配，只要URL地址的头部和path一致，组件就会挂载，不会检查子路径。

- 属性
  - path --> 要映射的URL地址
  - component --> 要挂载的组件
  - exact --> 路径是否**完整**匹配，默认值为false

```javascript
function App() {
  return (
    <div className="App">
        这是app
        <Route exact path='/' component={Home}/>
        <Route exact path='/Demo' component={Demo}/>
    </div>
  );
}
```

### 4. Link和NavLink

在页面中创建超链接，常规的做法是使用<a></a>，它会自动向服务器发送请求并重新加载页面。但是，使用router的话不能这么做。

##### 4.1 Link

```javascript
// 原来的方法
<a href='/'>Home</a>

// router方法（不会跳转）（在头部引入Link）
<Link to='/'>Home</Link>
```

##### 4.2 NavLink（可以指定样式）

比普通Link多了一个功能：在链接被激活时，它可以自动指定`activeClass`

```javascript
<NavLink activeClassName={classes.active} to='/'>Home</NavLink>
```

```css
a.active{
  color: red;
}
```

以上代码的作用：当链接被激活（active）时，字体颜色变为红色。



除了外部css样式，也可以用内联样式：

```javascript
<NavLink activeStyle={{color: 'red'}} to='/'>Home</NavLink>
```

### 5. 两种Router

开发过程中两者是没有区别的，但是在项目部署时有区别。



项目部署时要用到服务器，此处以`Nginx`为例：



​	使用==BrowserRouter==时直接通过URL地址进行组件的操作，使用过程中和普通的URL地址没有什么区别，但是刷新后会显示404 not found。



​	

​	**为什么会出现404？**

​		因为当通过Link构建跳转时，并没有向服务器发送请求，没有经过服务器，所以当刷新页面时（也就是向服务器发送请求）会找不到对应的文件，所以会显示404。



​	

​	**怎么解决？**

​	有两种解决方法：

1. 改用==HashRouter==

   服务器不会去判断hash值，使用HashRouter后请求会由router来处理

2. 修改服务器配置文件（非react方法）

   将所有请求都发送到index.html

   1. 打开服务器配置文件：

<img src="/Users/liuqiwei/Library/Application Support/typora-user-images/image-20231030154757872.png" alt="image-20231030154757872" style="zoom:70%;" />

​			2. 修改：

​			注掉第二行，添加上`try_files $uri /index.html;`



​	实际开发中，选择哪个router还是要看实际情况如何。

### 6. Route组件

 ##### 6.1 使用component指定要挂载的组件

component用来指定路由匹配后被挂载的组件，它直接**传递组件的类**，**无法自定义**里面要传递的参数。通过component构建的组件会自动被创建并自动传递参数。



传递的参数包含三种：

- ==match== -- 匹配的信息

  - isExact -- 检查路径是否完全匹配 

  - params -- 请求的参数，使用路由，可以根据不同的参数加载不同的数据（下面的代码以数据的id为例）

    `<Route path='/student/:id'/>`

- ==location== -- 地址信息（e.g. hash值、字符串、地址...）

- ==history== -- 历史记录，控制页面的跳转

  - `push()`：向历史记录中添加一条新的记录，跳转页面。从A跳到B，是真实的跳转，可以从B回退到A。
  - `replace()`：也是跳转，但是是替换页面。用B替换A，回退的话是回到A前面的页面而不是A（不会产生新的记录）

  <img src="/Users/liuqiwei/Library/Application Support/typora-user-images/image-20231031120035296.png" alt="image-20231031120035296" style="zoom:70%;" />

##### 6.2 使用render指定

和component的使用方法一样，但作用范围上是不同的。`render`需要一个回调函数作为参数，回调函数的返回值被挂载，返回值传什么，最终就会显示什么（一般是组件）。



⚠️注意：`render`不会自动传递match、location、history



​	**如何在使用render时传递参数？**

​	在render的回调函数中使用`routePros`，返回值中调用`routePros.match/location/history`，或者直接使用：`<Student {...routePros}/>`

##### 6.3 使用children指定

有两种用法：

1. 和render类似，传递回调函数。写法和render一模一样，只不过是将render换成了children。但是存在缺陷：设置了回调函数，即使路径不匹配，组件也会挂载。这种问题适合特殊场景，比如某些动画等。

   ```javascript
   <Route path='/student' children={(routePros)=>{<Student {...routePros}/>}}/>
   ```

   

2. 传递JSX，可以直接传递组件，但是无法自动获取到上面的三个属性 --> 额外通过Hook函数获取

   ```javascript
   // 设置钩子函数
   const match = useRouteMatch();
   const location = useLocation();
   const history = useHistory();
   
   // 挂载组件
   <Route path='/student/:id'>
     {routePros => <Student {...routePros}/>}
   </Route>
   ```

### 7. 路由嵌套

1. 手动编写

   手动编写访问路径，子组件的嵌套可以放在子组件页面上，也可以放在父组件页面上（推荐方式）

2. 使用`useRouteMatch()`动态获取路径

   ```javascript
   // 动态获取到路径前缀
   const {path} = useRouteMatch();
   
   // 使用模板字符串代替这个路径前缀
   <Route path={`${path}/hello`}>
   	<Hello/>
   </Route>
   ```

   



















