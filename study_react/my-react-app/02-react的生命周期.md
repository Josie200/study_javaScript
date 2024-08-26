虚拟 DOM：核心算法的基石

组件初始化： reder -> 生成虚拟 DOM -> ReactDOM.render() -> 真实 DOM

组件更新：render() -> 生成新的虚拟 DOM -> diff 算法 -> 定位出两次虚拟 DOM 的差异

React15 生命周期：
组件挂载：初始化渲染 -> constructor() -> componentWillMount() -> render() -> componentDidMount()

constructor()//挂在时调用一次
componentWillReceiveProps()//父组件修改组件的 propos 时会调用，如果父组件导致组件重新渲染，即使 props 没有更改，也会调用此方法
shouldComponentUpdate()//组件更新时调用
componentWillMount() //初始化渲染时调用
componentWillUpdate()//组件更新时调用
componentDidUpdate()//组件更新后调用
componentDidMount()//初始化渲染时调用
render()//在执行过程中并不会去操作真实 DOM，它的职能是把需要渲染的内容返回出来
componentWillUnmount()//组件卸载时调用

React16 生命周期：
组件挂载：初始化渲染 -> constructor() -> getDerivedStateFromProps() -> render() -> componentDidMount()
注：React 16 对 render 方法也进行了一些改进。React16 之前，render 方法必须返回单个元素，而 React 16 允许我们返回元素数组和字符串。
constructor()//挂在时调用一次
getDerivedStateFromProps()//初始化/更新时调用
componentDidMount()//初始化渲染时调用
shouldComponentUpdate()//组件更新时调用
getSnapshotBeforeUpdate()//组件更新前调用，可以获取组件的快照
componentDidUpdate()//组件更新后调用
render()//在执行过程中并不会去操作真实 DOM，它的职能是把需要渲染的内容返回出来
componentWillUnmount()//组件卸载时调用
