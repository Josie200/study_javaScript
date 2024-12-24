使用 shouldComponentUpdate 规避冗余的更新逻辑
PureComponent + Immutable.js
React.memo 与 useMemo

**shouldComponentUpdate**
React 组件会根据 shouldComponentUpdate 的返回值来决定是否执行该方法之后的生命周期进而决定是否对组件进行 re-render （重渲染）
shouldComponentUpdate 的默认值为 true 也就是说“无条件 re-render”

```jsx
import React from "react";

//ChildA组件
export default class ChildA extends React.Component {
    render() {
        console.log("ChildA 的render方法执行了");
    return (<div className="childA">子组件A的内容：{this.props.text}</div>);}}

//ChildB组件
import React from "react";
export default class ChildB extends React.Componentrender {
    render(){
     shouldComponentUpdate(nextProps) {
    // 比较当前的 props 和下一个 props，决定是否更新
    if (nextProps.text === this.props.text) {
      // 如果 text 没有变化，返回 false，不重新渲染
      return false;
    }
    // 否则，返回 true，允许重新渲染
    return true;
  }
    console.log("ChildB 的render方法执行了");
    return(<div className="childB">子组件B的内容：{this.props.text}</div>);}}

//父组件
import React from "react";
import ChildA from './ChildA'
 import ChildB from './ChildB'
 class App extends React.Component {
    state = {textA:'我是A的文本',textB: '我是B的文本'}changeA = () =>{this.setState({textA: 'A的文本被修改了'})}
 changeB = () =>{ this.setState({textB: 'B的文本被修改了'})}
 render() {return (
    <div className="App">
    <div className="container">
    <button onClick={this.changeA}>点击修改A处的文本</button>
    <button onClick={this.changeB}>点击修改B处的文本</button>
    <ul><li><ChildA text={this.state.textA}/></li>
    <li><ChildB text={this.state.textB}/></li>
    </ul></div></div>)}}
export default App;
```

**PureComponent + Immutable.js**
PureComponent:提前帮你安排好更新判定逻辑
shouldComponentUpdate 虽然一定程度上帮我们解决了性能方面的问题但在写了不计其数个 shouldComponentUpdate 逻辑之后,难免会怀疑人生
React 15.3 新增了一个叫 PureComponent 的类
PureComponent 将会在 shouldComponentUpdate 中对组件更新前后的 props 和 state 进行浅比较并根据浅比较的结果，决定是否需要继续更新流程

数据类型为引用类型
若数据内容没变，但是引用变了那么浅比较仍然会认为“数据发生了变化”，进而触发一次不必要的更新导致过度渲染
若数据内容变了，但是引用没变那么浅比较则会认为“数据没有发生变化”，进而阻断一次更新导致不渲染

Immutable: “不可变值”让“变化”无处遁形
PureComponent 浅比较带来的问题本质上是对“变化”的判断不够精准导致的

```jsx
// 引入 immutable 库里的 Map 对象，它用于创建对象
import { Map } from "immutable";
//初始化一个对象 baseMap
const baseMap = Map({ name: "修言", career: "前端", age: 99 });
//使用 immutable 暴露的 Api 来修改 baseMap 的内容
const changedMap = baseMap.set({ age: 100 });
//我们会发现修改 baseMap 后将会返回一个新的对象，这个对象的引用和baseMap 是不同的
console.log("baseMap === changedMap", baseMap === changedMap);
```

**React.memo 与 useMemo**

使用 React.memo 来包裝函数组件

```jsx
export default React.memo(FunctionDemo, areEqual);
```

React.memo 可以实现类似于 shouldComponentUpdate 或者 PureComponent 的效果
如果希望复用的是组件中的某一个或几个部分这种更加“精细化”的管控，就需要 useMemo 来帮忙了
