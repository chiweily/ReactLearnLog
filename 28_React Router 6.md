# 28 Router 6

### 1. 路由映射

路径访问默认是严格匹配（v5不是）



新增加了组件<Routes/>，用于Route的容器，Routes中的Route只有一个会被匹配，并且需要`element`来指定要挂载的组件。

```javascript
<Routes>
  <Route path='/' element={<Home/>}>
  </Route>
</Routes>
```



- v5中的~~component、render~~在v6中不存在，新增加了element，它直接传递JSX。

- 可以自动补全路径

### 2. Link

link无变化，和v5一样

整体逻辑是，先link，后routes

### 3. 参数和Hook

可以用`useParams()`获取参数

```javascript
const {id} = useParams();
```



- match --> `useMatch()`，用来检查当前url是否匹配某个路由

  ```javascript
  // 如果路径匹配，则返回一个对象，不匹配则返回null
  const match = useMatch('/student/:id');
  ```

  

- location --> `useLocation()`，获取当前的地址信息

- history --> 用`useNavigate()`替代了useHistory()，它返回一个navigate对象。作用：获取一个用于跳转页面的函数（和history的作用一样）

  ```javascript
  // 使用navigate跳转页面
  const nav = useNavigate();
  
  // 直接调用nav跳转至home页面
  const clickHandler = () => {
    // nav('/home');  // 默认使用push方法
    
    // 使用replace方法的话，需要对nav进行配置
    nav('/home', {replace: true});
  };
  ```

### 4. 嵌套路由和Outlet

##### 4.1 嵌套路由

 将所有路由写在一起，用<Routes></Routes>将所有<Route>包在一起，<Route>标签按层级来写

```javascript
<Routes>
  <Route path='/' element={<Home/>}></Route>
  <Route path='about' element={<About/>}>
    <Route path='hello' element={<Hello/>}></Route>
  </Route>
</Routes>
```

##### 4.2 Outlet

子路径不用写全，在要进行嵌套的页面中写<Outlet/>，它起到一个类似“占位符”的作用

- 当嵌套路由中的路径匹配成功了，outlet则表示嵌套路由中的组件
- 如果没有成功，outlet就什么都不是

### 5. Navigate

用来跳转到指定的位置，默认使用push跳转（所以会产生新的历史记录）

如果想用replace的话，这样写：

`<Navigate to='/student/1' replace/>`

### 6. NavLink

相比普通的<Link>，<NavLink>多了可以设置样式的设定，不同于v5，v6中的样式设定是以回调函数的形式呈现的，返回值是设定的样式

```javascript
<NavLink
  style={
    ({isActive}) => {
      return isActive?
        {color: 'red'} : null
    }
  }
  to='/student/2'>学生</NavLink>
```













































