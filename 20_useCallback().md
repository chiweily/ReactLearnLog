# 20 useCallback()

将想稳定的回调函数写到`useCallback()`里作为第一个参数，然后将回调函数所依赖的<font color='red'>**所有变量**</font>作为数组，也就是第二个参数，`setState()`不用这样设置。

只有当依赖数组发生变化时，函数才会重新创建。如果不指定依赖数组，那么回调函数每次都会重新创建。

```JavaScript
const [count, setCount] = useState(1);

const clickHandler = useCallback(() => {
    setCount(prevState => prevState + 1);
}, []);
```

### 1. 定义

是一个钩子函数，用来创建 react 中的回调函数。它创建的回调函数不会总在组件重新渲染时重新创建。

### 2. `useCallback()`和`useMemo()`的区别？

1. 用途
   1. `useCallback` 用于优化回调函数，确保它们在依赖不变的情况下稳定。通常用于传递回调函数给子组件，以避免不必要的子组件重新渲染。
   2. `useMemo` 用于计算并缓存值，以确保在依赖不变的情况下稳定。通常用于计算和缓存昂贵的计算结果，例如从 props 计算派生状态。
2. 参数
   1. `useCallback` 接受两个参数：回调函数和依赖项数组。
   2. `useMemo` 接受两个参数：一个函数和依赖项数组。该函数用于计算要缓存的值。
3. 返回值
   1. `useCallback` 返回稳定的回调函数
   2. `useMemo` 返回计算的值