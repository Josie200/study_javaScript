BrowserRouter 路由器，比如 BrowserRouter 和 HashRouter
Route 路由，比如 Route 和 Switch
Link 导航，比如 Link、NavLink、Redirect
路由（以 Route 为代表）负责定义路径与组件之间的映射关系
导航（以 Link 为代表）负责触发路径的改变
路由器则会根据 Route 定义出来的映射关系为新的路径匹配它对应的逻辑

**BrowserRouter 和 HashRouter**
createBrowserHistory BrowserRouter 是使用 HTML 5 的 history API 来控制路由跳转的
createHashHistory HashRouter 是通过 URL 的 hash 属性来控制路由跳转的
