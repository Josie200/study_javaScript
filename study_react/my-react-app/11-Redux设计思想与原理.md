**Flux**
可以认为 Redux 是 Flux 的一种实现形式
Flux 并不是一个具体的框架它是一套由 Facebook 技术团队提出的应用架构这套架构约束的是应用处理数据的模式

**认识 Flux**
核心特征是**单向数据流**

1. View(视图层) 用户界面该用户界面可以是以任何形式实现出来的 Flux 架构与 React 之间并不存在耦合关系
2. Action(动作) 可以理解为视图层发出的“消息”它会触发应用状态的改变
3. Dispatcher(派发器) 它负责对 action 进行分发
4. Store(数据层) 它是存储应用状态的“仓库”此外还会定义修改状态的逻辑

**Redux 关键要素与工作流**
Store：它是一个单一的数据源，而且是只读的
Action：是“动作”的意思，它是对变化的描述
Reducer:它负责对变化进行分发和处理,最终将新的数据返回给 Store

**createStore**

1. 调用 createStore
2. 处理没有传入初始状态（前两个入参都为 function）的情况
3. 若 enhancer 不为空，则用 enhancer 包装 createStore
4. 定义内部变量
5. 定义 ensureCanMutateNextListeners 方法该方法用于确保 currentListeners 与 nextListeners 不指向同一个引用
6. 定义 getState 方法，该方法用于获取当前的状态
7. 定义 subscribe 方法,该方法用于注册 listeners (订阅监听函数)
8. 定义 dispatch 方法，该方法用于派发 action、调用 reducer 并触发订阅
9. 定义 replaceReducer 方法,该方法用于替换 reducer
10. 执行一次 dispatch,完成状态的初始化
11. 定义 observable 方法（此处可忽略）
12. 将步骤 6~11 中定义的方法放进 store 对象中返回

**dispatch**

1. 调用 dispatch，入参为 action 对象
2. 前置校验
3. “上锁”：将 isDispatching 置为 true
4. 调用 reducer，计算新的 state
5. “解锁”：将 isDispatching 置为 false
6. 触发订阅
7. 返回 action
   Redux 首先会将 isDispatching 变量置为 true 待 reducer 执行完毕后再将 isDispatching 变量置为 false 用 isDispatching 将 dispatch 的过程锁起来是为了避免开发者在 reducer 中手动调用 dispatch

**Redux 种的发布订阅模式 subscribe**

1. 调用 subscribe，入参是一个函数
2. 前置校验
3. 调用 ensureCanMutateNextListeners 确保 nextListeners 与 currentListeners 不指向同一个引用
4. 注册监听函数，将入参 listener 函数推入 nextListeners 数组中
5. 返回取消订阅当前 listener 的方法(unsubscribe)
   subscribe 并不是一个严格必要的方法只有在需要监听状态的变化时我们才会调用 subscribe
6. 在 store 对象创建成功后通过调用 store.subscribe 来注册监听函数
7. 当 dispatch action 发生时 Redux 会在 reducer 执行完毕后将 listeners 数组中的监听函数逐个执行
