**原生 DOM 下的事件流**
W3C 标准约定了一个事件的传播过程要经过以下 3 个阶段
事件捕获阶段
目标阶段
事件冒泡阶段
DOM 事件流下的性能优化思路：事件委托
把多个子元素的同一类型的监听逻辑合并到父元素上通过一个监听函数来管理的行为
将事件绑定在父元素上

```html
<ul id="poem">
  <li>a</li>
  <li>v</li>
  <li>b</li>
  <li>n</li>
  <li>m</li>
  <li>e</li>
</ul>
```

```javascript
var ul = document.getElementByld("poem");
ul.addEventListener("click", function (e) {
  console.log(e.target.innerHTML);
});
```

React 时间系统工作拆解 1.事件绑定:事件的绑定是在 completeWork 中完成的，completeWork 内部有三个关键动作：创建 DOM 节点(createlnstance) 将 DOM 节点插入到 DOM 树中(appendAllChildren) 为 DOM 节点设置属性(finalizelnitialChildren) 2.事件触发:事件触发的本质是对 dispatchEvent 函数的调用

模拟事件在冒泡阶段的传播顺序,收集冒泡阶段相关的节点实例与回调函数
唯一的区别是对 path 数组的倒序遍历变成了正序遍历正序遍历自然模拟的就是冒泡阶段的事件传播顺序若该节点上对应当前事件的冒泡回调不为空那么节点实例和事件回调同样会分别被收集到 SyntheticEvent.\_dispatchInstances 和 SyntheticEvent.\_dispatchListeners 中去
