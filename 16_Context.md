# 16 Context

提供了一种在不同组件间共享数据的方式，不用一层一层地访问，可以让组件直接访问到数据。在context中设置要“全局访问”的数据，然后就能在所有组件中访问到该数据（类似“全局作用域”）

### 1. 使用方式1

在父组件中直接return以下代码：

`<XXX.Consumer>`

XXX是context的组件名称，这个组件必须传递一个回调函数，然后它会将创建的context作为参数传递到这个回调函数中。这样通过这个被传递的参数就能访问到context中存储的数据。

```javascript
React.reateContext();

	<MyContext.Provider>...
	<MyContext.Consumer>...
```

### 2. 使用方式2（常用）<span style="background-color: #F2C1BE">【函数式组件】</span>

1. 导入context
2. 使用hook，`useContext()`获取到context

1. `useContext()`需要一个Context作为参数，它会获取到Context中的数据，并将其作为返回值返回

```javascript
// 在父组件中，TestContext是context组件名
const demo = useContext(TestContext);
```

3. Provider & Consumer
   1. xxx.Provider: 表示数据的生产者，用value来指定Context中存储的数据。这样，该组件所有的子组件中都可以通过Context来访问指定的数据
   2. 当通过context访问数据时，内部的组件会读取离它最近的provider中的数据（由近到远）；如果provider中没有value，那就读取context中的默认数据

1. 