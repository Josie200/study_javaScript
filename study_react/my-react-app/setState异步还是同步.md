setState 的表现会因调用场景的不同而不同：
在 React 钩子函数及合成事件中，它表现为**异步**
在 setTimeout、setInterval 等函数中包括在 DOM 原生事件中,它都表现为**同步**
