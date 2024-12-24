高阶组件(HOC)
Render Props
剥离有状态组件与无状态组件

**高阶组件(HOC)**
高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式

```jsx
const withProps = (WrappedComponent) => {
  const targetComponent = (props) => (
    <div className="wrapper-container">
      <WrappedComponent {...props} />
    </div>
  );
  return targetComponent;
};
```

高阶组件不仅能够帮助我们简化逻辑的引入过程还可以帮助我们规避掉逻辑变更带来的烦琐的修改步骤

**Render Props**
术语 “render prop”是指一种在 React 组件之间使用一个值为函数的 prop 共享代码的简单技术

高阶组件的使用姿势是用“函数”包裹“组件”而 render props 恰恰相反它强调的是用“组件”包裹“函数”

```jsx
import React from "react";
const RenderChildren = (props) => {
  return <React.Fragment>{props.children(props)}</React.Fragment>;
};

<RenderChildren>{() => <p>我是 RenderChildren 的子组件</p>}</RenderChildren>;
```

函数并不一定要作为子组件传入,它也可以以任意属性名传入只要 render props 組件可以感知到它就行
在软件设计模式中，有一个非常重要的原则叫“开放封闭原则”一个好的模式,应该尽可能做到对拓展开放,对修改封闭
处理同样的需求 render props 就能够在保全“开放封闭”原则的基础上帮我们达到目的

**有状态组件与无状态组件**
函数组件顾名思义，就是以函数的形态存在的 React 组件早期并没有 React-Hooks 的加持，函数组件内部无法定义和维护 state 因此它还有一个别名叫“无状态组件”

```jsx
//无状态组件
function DemoFunction(props) {
  const { text } = props;
  return (
    <div className="demoFunction">
      <p>{`function 组件所接收到的来自外界的文本内容是： [${text}]`}</p>
    </div>
  );
}

//有状态组件
import React, { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  const increment = () => {
    setCount(count + 1); // 更新状态
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
    </div>
  );
}

export default Counter;
```

按照“单一职责”的原则我们应该将数据处理的逻辑和界面渲染的逻辑剥离到不同的组件中去这样功能模块的组合将会更加灵活，也会更加有利于逻辑的复用
