# 核心架构

react核心可以用 UI = fn(state) 来表示，把网页的呈现看成一个函数处理过后的状态呈现；
```js
const state = reconcile(update);
const UI = commit(state);
```

# 源码模块
- 调度器 Scheduler，负责任务优先级，暂停和运行，让优先级高的任务先进行协调Reconcile；
- 协调器 Reconciler，负责协调Dom的特征变化；
- 渲染器 Renderer，负责最后呈现；

<https://www.cnblogs.com/axl234/p/16063280.html>
<https://www.cnblogs.com/axl234/p/16027155.html>