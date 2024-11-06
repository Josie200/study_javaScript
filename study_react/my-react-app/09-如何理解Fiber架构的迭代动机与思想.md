JavaScript 对主线程的超时占用问题
Stack Reconciler 是一个同步的递归过程

**react16 的 Fiber 架构**
架构核心：可中断 可恢复 优先级
从架构角度来看，Fiber 是对 React 核心算法的重写
从编码角度来看，Fiber 是 React 内部所定义的一种数据结构
从工作流的角度来看，Fiber 节点保存了组件需要更新的状态和副作用

react16 之前：Reconciler->Renderer
react16 之后：Scheduler->Reconciler->Renderer

**reactDom.render() 调用栈--初始化阶段**
performSyncWorkOnRoot 是 render 阶段的起点
render 阶段的任务就是完成 Fiber 树的构建
它是整个渲染链路中最核心的一环

1. performSyncWorkOnRoot
   作用：这个函数是 React 渲染过程中同步工作的一部分。它会在 Root Fiber 上执行工作，通常是开始渲染过程。
   功能：它触发 React 内部的同步更新和渲染流程，启动工作循环来执行渲染任务。它常常由 React 调度器调用，主要是同步执行更新。
2. workLoopSync
   作用：workLoopSync 是一个工作循环，它会按顺序执行一系列任务，直到所有任务都完成或达到最大时间限制。
   功能：这个函数是同步工作流的一部分，主要用于处理已准备好执行的 Fiber 树上的所有工作。它确保所有操作按照同步的方式完成，直到渲染结束或达到上限。
   执行：它会遍历待处理的工作队列，并执行相关操作，比如更新状态、执行生命周期方法、渲染新的 UI 等。
3. performUnitOfWork
   作用：performUnitOfWork 是实际执行单个单位工作的函数，在 Fiber 架构中，这意味着在渲染树中按顺序执行每个节点。
   功能：它负责对当前的 Fiber 节点进行工作处理，更新 DOM 或执行计算。每次执行完一个 unit of work 后，它会决定是否继续处理下一个 Fiber。
   执行：在 workLoopSync 中，React 会一次处理一个 Fiber 节点，直到整个工作单元被完成。
4. beginWork
   作用：beginWork 是 React 渲染中开始执行工作的核心函数之一，它是每个 Fiber 节点的开始工作阶段。
   功能：在每个 Fiber 上执行的函数，它根据当前 Fiber 的类型（如功能组件、类组件或原生 DOM 元素）决定该做什么工作。它会决定是否需要继续渲染子节点、更新 props 或状态。
   执行：它是 Fiber 渲染流程中的关键步骤，通常是触发更新、处理副作用或执行计算。
5. completeWork
   作用：completeWork 是 React 渲染中每个 Fiber 节点的“完成”阶段的核心函数。
   功能：在这个阶段，React 处理组件渲染结果并将它们提交给父节点或更高层级。它标志着对当前 Fiber 节点的处理已经完成，并且可以继续处理父级节点。
   执行：一旦完成一个节点的工作，React 会执行必要的副作用，比如触发 DOM 更新、提交到渲染队列或进行 DOM 操作。
6. completeUnitOfWork
   作用：completeUnitOfWork 是处理完成一个工作单元后执行的函数。
   功能：当 performUnitOfWork 处理完当前 Fiber 后，它会调用 completeUnitOfWork，继续处理渲染中的父节点或其他操作。它确保整个工作单元（包括所有子节点）的渲染被完整处理。
   执行：该阶段可以涉及合并更新结果、处理副作用、将渲染结果应用到 DOM 等。
7. reconcileChildFibers
   作用：reconcileChildFibers 是 React 中的一个函数，用来处理和协调子 Fiber 节点。
   功能：它会检查当前 Fiber 节点的子节点，决定如何更新、添加、删除或重新排序子节点。这是 React 在执行渲染时非常重要的一步，确保 UI 树的变化能够高效地更新。
   执行：在 beginWork 过程中，React 会通过 reconcileChildFibers 计算出哪些子节点需要更新，并返回一个新的 Fiber 树。这个过程涉及到 Fiber 树的比较、差异化计算（类似于虚拟 DOM 的 diff 算法）。
   总结
   这些函数和步骤属于 React Fiber 架构中的调度和渲染流程。它们共同作用来确保 React 能够高效地渲染 UI，并处理所有组件的更新、插入、删除等操作。React 使用这些函数来执行细粒度的工作任务，以便能尽可能地优化渲染性能，同时确保 UI 的一致性。

performSyncWorkOnRoot：启动同步工作流程。
workLoopSync：执行工作循环，处理待更新的 Fiber。
performUnitOfWork：处理单个工作单元（Fiber 节点）。
beginWork：开始执行一个 Fiber 节点的工作。
completeWork：完成当前 Fiber 的工作。
completeUnitOfWork：完成一个工作单元。
reconcileChildFibers：协调和处理子节点的更新。
每个函数的作用是确保更新工作流的有效推进，优化渲染性能和保证 UI 的响应速度。
**双缓冲**
在 React 中,双缓冲模式的主要利好则是能够帮我们较大限度地实现 Fiber 节点的复用从而减少性能方面的开销

current 树与 workInProgress 树可以对标“双缓冲”模式下的两套缓冲数据:当 current 树呈现在用户眼前时,所有的更新都会由 workInProgress 树来承接 worklnProgress 树将会在用户看不到的地方（内存里）悄悄地完成所有改变
