Jsx 是如何映射为 DOM 的？
JSX 就是 React.createElement()的语法糖，它可以将 JSX 代码转换为 React.createElement()函数调用。

例如:

JS 代码：

```jsx
<ul className="list">
  <li key="2">1</li>
  <li key="2">2</li>
</ul>
```

转换成 JSX 代码:

```jsx
React.createElement("ul",{
//传入属性键值对
className: "list"// 从第三个入参开始往后，传入的参数都是 children
},
React.createElement("li", { key: "1"}, "1"),
React.createElement("li", { key: "2" }, "2"));

ReactDOM.render(需要渲染的元素（ReactElement)//element,元素挂载的目标容器（一个真实 DOM）container,回调函数，可选参数，可以用来处理渲染结束后的逻辑[callback]
```

JSX 代码 -> Babel 编译器 -> React.createElement()函数调用 -> ReactElement 调用 -> 虚拟 DOM -> ReactDOM.render()渲染 -> 真实 DOM
