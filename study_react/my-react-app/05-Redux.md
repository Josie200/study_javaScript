**Redux 是 JavaScript 状态容器，它提供可预测的状态管理。**

1.store 是一个单一的数据源，而且是只读的
2.action 是对变化的描述
3.reducer 负责对变化进行分发和处理

在 redux 的整个工作过程中，数据流是严格单向的
action -> reducer -> store -> view - action

```jsx
// 引入 redux
import { createStore } from "redux";
const reducer = (state, action) => {
  return new_state;
};
// 创建 store
const store = createStore(reducer);

const action = { type: "ADD_ITEM", payload: 1 };

store.dispatch(action); // 触发 action
```
