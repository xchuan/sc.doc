# React Hooks List

### v16
- useState Hook，常用于单个状态值的管理，基本理解
- useEffect Hook，有两个变种，useLayoutEffect和useInsertionEffect；基本理解
- useRef Hook，常用于保存一些不需要渲染的值，例如DOM元素节点或timeout ID，更新ref不会重新渲染组件，基本理解
- useContext Hook，读取订阅上下文
- useReducer Hook，常用于负责状态的管理，入参有2个，返回的前两个值为state和dispatch；说明：useReducer 是 useState 的一种代替方案，用于<u> state 之间有依赖关系</u>或者比较复杂的场景。

<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/wgnbmtjb-the-looper/embed/wvOGNyY?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/wgnbmtjb-the-looper/pen/wvOGNyY">
  Untitled</a> by 单飞浪子 (<a href="https://codepen.io/wgnbmtjb-the-looper">@wgnbmtjb-the-looper</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

# 性能相关Hook
- React.memo，包裹不需要重新渲染的组件，使用场景当父组件重新渲染，没必要重新渲染的子组件可以使用memo包起来

```js
function Child(props){
  console.log('Child');
  return (
    <button>Child</button>
  )
}
// memo：如果你想让一个函数组件有一个功能，如果属性不变，就不要刷新。
Child=React.memo(Child);
function App() {
  let [name,setName]=React.useState('zhufeng');
  return (
    <div> 
      /* 在input框中输入内容，会走setName导致App组件重新渲染，但是子组件Child也会进行渲染。 */
      /* 解决：子组件使用memo包起来 */
      <input type="text" value={name} onChange={ev=>setName(ev.target.value)} />
      <Child/>
    </div>
  )
}
```
原理
```js
function memo(OldComponent) {
  return class extends React.PureComponent {
    render() {
      return <OldComponent {...this.props} />
    }
  }
}
```

- useMemo Hook，两个入参，第一个高开销计算函数，第二个依赖的值可以是从props，state和组件中定义的变量和函数
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

- useCallback Hook，可以理解为将函数进行了缓存；useMemo 返回是新数据对象，而 useCallback 返回是回调函数。

<iframe height="300" style="width: 100%;" scrolling="no" title="Untitled" src="https://codepen.io/wgnbmtjb-the-looper/embed/preview/jOJqJbE?default-tab=html%2Cresult" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href="https://codepen.io/wgnbmtjb-the-looper/pen/jOJqJbE">
  Untitled</a> by 单飞浪子 (<a href="https://codepen.io/wgnbmtjb-the-looper">@wgnbmtjb-the-looper</a>)
  on <a href="https://codepen.io">CodePen</a>.
</iframe>

- useImperativeMethods，是子组件向父组件暴露方法或数据的Hook，一般和forwardRef配合使用

```js
//子组件
import React, { useImperativeHandle, useState,forwardRef } from 'react';
const ListChild = (props) => {
    const [count,setcount]=useState(0)
    const {onRef}=props
    // 利用useImperativeHandle将子组件内的数据暴露给父组件
    //onRef 为父组件向子组件传的参数值
    useImperativeHandle(onRef, () => {
        return {
            init,
            count
        }
    })
    const init=()=>{
        setcount(2)
        console.log('我是子组件内地方法')
    }
    return (
        <div>
            <h2>{count}</h2>
        </div>
    )
}

export default forwardRef(ListChild)

//父组件
import React, {useState,useRef} from 'react'
import './index.css'
import ListChild from './listChild/list1'
import {Button} from "antd";
const List = (props) => {
    const handleCLick=()=>{
        onSpecUserRef&&onSpecUserRef.current.init()
    }
    const onSpecUserRef = useRef()
  return (
    <div>
        <ListChild     onRef={onSpecUserRef}/>
        <Button onClick={handleCLick}>点击加1</Button>
    </div>
  )
}

export default List
```

- useLayoutEffect，使用场景：useLayoutEffect 会在 浏览器 layout 之后，painting 之前执行。
```js
import React from 'https://esm.sh/react@18';
import ReactDom from 'https://esm.sh/react-dom@18';
import { message } from "https://esm.sh/antd@5.12.5?bundle&deps=react@18.2.0";
const {useMemo, useState, useEffect, useRef, useLayoutEffect} = React;
const BlinkyRender = () => {
  const [value, setValue] = useState(0);
  //useLayoutEffect
  useLayoutEffect(() => {
    if (value === 0) {
      setValue(10 + Math.random() * 200);
    }
  }, [value]);
  return (<>
    <div>value: {value}</div>
    <input type="button" onClick={() => setValue(0)} value="确定" />
  </>);
};
function App({title}) {
	return(<>
		<h1>乱跳问题：</h1>
    <BlinkyRender />
	</>);
}
ReactDom.render(
  <App />,document.querySelector("#root")
)
```

- useMutationEffect，废弃
- useDebugValue，可用于在 React 开发者工具中显示自定义 hook 的标签

# 手写一个useMemo
```js
const useMemo = (memoFn, dependencies) => {
  if (memorizedState[index]) {
    // 不是第一次执行
    let [lastMemo, lastDependencies] = memorizedState[index]

    let hasChanged = !dependencies.every((item, index) => item === lastDependencies[index])
    if (hasChanged) {
      memorizedState[index++] = [memoFn(), dependencies]
      return memoFn()
    } else {
      index++
      return lastMemo
    }
  } else {
    // 第一次执行
    memorizedState[index++] = [memoFn(), dependencies]
    return memoFn()
  }
}
```

参考资料:
<https://jacky-summer.github.io/2020/10/26/%E6%89%8B%E5%86%99%E6%A8%A1%E6%8B%9F%E5%AE%9E%E7%8E%B0-React-Hooks/>


# v18 新Hook
- useTransition
- useMutableSource
- useDeferredValue
- useTransitionState
- useSyncExternalStore
- useImperativeHandle

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
<https://kuangpf.com/blog/2020/04/25/react-hooks/#useReducer>
<https://juejin.cn/post/7083308347331444750>
React Profiler vs React DevTools Profiler