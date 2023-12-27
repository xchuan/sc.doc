# React Hooks List
v16
useState Hook，常用于单个状态值的管理，基本理解
useEffect Hook，有两个变种，useLayoutEffect和useInsertionEffect；基本理解
useRef Hook，常用于保存一些不需要渲染的值，例如DOM元素节点或timeout ID，更新ref不会重新渲染组件，基本理解

useContext Hook，读取订阅上下文
useReducer Hook，常用于负责状态的管理，入参有2个，返回的前两个值为state和dispatch；（待研究）

# 性能相关Hook
React.memo，包裹不需要重新渲染的组件
useMemo Hook，两个入参，第一个高开销计算函数，第二个依赖的值可以是从props，state和组件中定义的变量和函数
注意useMemo 是return一个值出来，和useEffect不一样

示例
```js
import React, { useMemo } from 'react';
 
function ProductList({ products }) {
  // 使用 useMemo 记忆产品列表的生成结果
  const productList = useMemo(() => {
    return products.map((product) => (
      <li key={product.id}>{product.name}</li>
    ));
  }, [products]);
 
  return (
    <ul>
      {productList}
    </ul>
  );
}
```

通过使用useMemo，我们确保只有在products发生变化时才重新生成产品列表，从而减少了不必要的计算。
适用场景：
> 1. 当计算结果昂贵且在渲染中频繁使用时。
> 2. 当计算结果依赖于props或状态的变化时。
> 3. 当你想要避免不必要的计算开销。

useCallback Hook
useMemo 返回是新数据对象，而 useCallback 返回是回调函数。

# 手写一个useMemo

# v18 新Hook
useTransition
useMutableSource
useDeferredValue
useTransitionState
useSyncExternalStore
useImperativeHandle

# React 性能监控
React Profiler

```js
import React, { Profiler } from 'react';

function MyComponent() {
  return (
    <div>
      <h1>Hello, World!</h1>
    </div>
  );
}

function onRenderCallback(
  id, // 组件的标识符
  phase, // "mount"（如果组件刚刚挂载）或 "update"（如果组件重新渲染）
  actualDuration, // 渲染本身花费的时间
  baseDuration, // 估计不使用memoization的情况下渲染整个子树需要的时间
  startTime, // 本次更新中React开始渲染的时间
  commitTime, // 本次更新中React committed的时间
  interactions // 属于本次更新的 interactions 的集合
) {
  console.log(`${id} ${phase} took ${actualDuration} ms`);
}

function App() {
  return (
    <Profiler id="MyComponent" onRender={onRenderCallback}>
      <MyComponent />
    </Profiler>
  );
}
```

参考网站：
<https://juejin.cn/post/7263719253916041274>
<https://github.com/PDKSophia/blog.io/blob/master/%E5%89%8D%E7%AB%AF/React/React%E5%90%88%E6%88%90%E4%BA%8B%E4%BB%B6%E7%9A%84%E8%83%8C%E5%90%8E%E6%95%85%E4%BA%8B.md>

React Profiler vs React DevTools Profiler