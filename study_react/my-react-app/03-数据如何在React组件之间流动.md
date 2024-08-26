基于 props 的单向数据流
单向数据流：
当前组件的 state 以 props 的形式流动时只能流向组件树中比自己层级更低的组件

```javascript
//当层级太多时，不推荐使用props，会增加后期维护的成本，推荐使用发布订阅模式
function Child(props) {
  return (
    <div className="child">
      <p>{`子组件所接收到的来自父组件的文本内容是：${props.fatherText}`}</p>
      <button onClick={props.changeText}></button>
    </div>
  );
}

class Father extends React.Component {
  // 初始化父组件的
  state = { text: "初始化的父组件的文本" };
  //按钮的监听函数，用于更新 text 值
  changeText = () => {
    this.setState({ text: "改变后的父组件文本" });
  };

  render() {
    return (
      <div className="father">
        <button onClick={this.changeText}></button>
        <Child fatherText={this.state.text} changeText={this.changeText} />
      </div>
    );
  }
}
```

发布订阅机制
监听事件的位置和触发事件的位置是不受限的

```javascript
class myEventEmitter {
  constructor() {
    //eventMap用来存储事件和监听函数之间的关系
    this.eventMap = {};
  }
  //type这里就代表事件名称
  on(type, handler) {
    //handler必须是一个函数，如果不是直接报错
    if (!(handler instanceof Function)) {
      throw new Error("第二个参数必须是一个函数");
    }
    //判断type事件对应的队列是否存在
    if (!this.eventMap[type]) {
      //若不存在，新建该队列
      this.eventMap[type] = [];
    }
    //若存在，直接往队列中推入handler
    this.eventMap[type].push(handler);
  }

  //触发时是可以携带数据的，data就是数据的载体
  emit(type, data) {
    //假设该事件是有订阅的（对应的事件队列存在）
    if (this.eventMap[type]) {
      //将事件队列里的 handler 依次执行出队
      this.eventMap[type].forEach((handler) => {
        //读取data
        handler(data);
      });
    }
  }

  off(type, handler) {
    if (this.eventMap[type]) {
      this.eventMap[type].splice(this.eventMap[type].indexOf(handler) >>> 0, 1); //>>> 0 会确保 splice 的行为不会错误地移除其他元素，因为 splice 会从一个无效的索引尝试删除，这样就不会删除任何东西。
    }
  }
}

const myEvent = new myEventEmitter();

const testHandler = function (data) {
  console.log(data);
};

myEvent.on("test", testHandler);

myEvent.emit("test", "hello world"); //输出 hello world
```
