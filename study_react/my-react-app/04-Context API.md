**旧的 Context API 存在的问题**

如果组件提供的一个 Context 发生了变化，而中间父组件的 shouldComponentUpdate 返回 false，那么使用到该值的后代组件不会进行更新。使用了 Context 的组件则完全失控，所以基本上没有办法能够可靠的更新 Context。

**新的 Context API 解决了这个问题，它允许组件订阅 Context 的变化，并在组件更新时自动重新渲染。**

即便组件的 shouldComponentUpdate 返回 false 它仍然可以“穿透”组件继续向后代组件进行传播进而确保了数据生产者和数据消费者之间数据的一致性
