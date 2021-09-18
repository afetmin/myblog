# React的diff

一个`DOM节点`在某一时刻最多会有4个节点和它相关。

1. `current Fiber`。如果该`DOM节点`已在页面中，`current Fiber`代表该`DOM节点`对应的`Fiber节点`。
2. `workInProgress Fiber`。如果该`DOM节点`将在本次更新中渲染到页面中，`workInProgress Fiber`代表该`DOM节点`对应的`Fiber节点`。
3. `DOM节点`本身。
4. `JSX对象`。即`ClassComponent`的`render`方法的返回结果，或`FunctionComponent`的调用结果。`JSX对象`中包含描述`DOM节点`的信息。

`Diff算法`的本质是对比1和4，生成2

## diff算法的优化

由于diff算法本身会带来性能损耗，前后两颗树完全比对的算法复杂度为O(n3)，其中n是树中元素的的数量。

为了降低算法复杂度，`React`的`diff`会预设三个限制：

1. 只对同级元素进行`Diff`。如果一个`DOM节点`在前后两次更新中跨越了层级，那么`React`不会尝试复用他。
2. 两个不同类型的元素会产生出不同的树。如果元素由`div`变为`p`，React会销毁`div`及其子孙节点，并新建`p`及其子孙节点。
3. 开发者可以通过 `key prop`来暗示哪些子元素在不同的渲染下能保持稳定。

考虑如下例子：

```html
// 更新前
<div>
  <p key="ran">ran</p>
  <h1 key="xiu">xiu</h1>
</div>

// 更新后
<div>
  <h1 key="xiu">xiu</h1>
  <p key="ran">ran</p>
</div>
```

如果没有`key`，`React`会认为`div`的第一个子节点由`p`变为`h1`，第二个子节点由`h1`变为`p`。这符合限制2的设定，会销毁并新建。

但是当我们用`key`指明了节点前后对应关系后，`React`知道`key === "ran"`的`p`在更新后还存在，所以`DOM节点`可以复用，只是需要交换下顺序。

reconcileChildFibers 函数会根据不同的newChild（JSX对象）调用不同的处理函数。

## 单节点diff

当newChild类型为object、number、string，代表同级只有一个节点。

1. 先判断key是否相同，然后type，都相同时DOM才能复用。
2. 当child !== null且key相同且type不同时，执行deleteRemainingChildren将child及其兄弟fiber都标记删除。
3. 当child !== null且key不同时，仅将child标记删除。

关于2，3步，当key相同但type不同，说明已经完全无法复用了，都需要删除。但key不同只代表该fiber不能复用，后面的兄弟fiber还有复用的可能性。

## 多节点diff

当newChild类型为Array，同级有多个节点。

多节点diff有多种情况需要处理

### 1. 节点更新

节点更新又包含两种情况：

```html
<div>
	<p key="p"></p>
</div>
// 属性变化
<div>
	<p key="p" className="p"></p>
</div>
// 类型变化
<div>
	<h1 key="p" className="p"></h1>
</div>
```

### 2. 节点新增或删除

```html
<div>
	<p key='1'>1</p>
	<p key='2'>2</p>
</div>

// 新增
<div>
	<p key='1'>1</p>
	<p key='2'>2</p>
	<p key='3'>3</p>
</div>
//删除
<div>
	<p key='1'>1</p>
</div>
```

### 3. 节点位置变化

```html
<div>
	<p key='1'>1</p>
	<p key='2'>2</p>
</div>

// 
<div>
	<p key='2'>2</p>
	<p key='1'>1</p>
</div>
```

不同的情况执行不同的逻辑，React团队发现更新比其他两种的频率更高，于是diff优先判断更新情况。又因为fiber是单链表结构的，所以无法使用双指针优化遍历。diff会经过两轮遍历：

- 第一轮：处理更新节点。
- 第二轮：处理不为更新的节点。

## React中触发更新

除了SSR相关，触发更新的方法：

- ReactDOM.render
- this.setState
- this.forceUpdate
- useState
- useReducer

### 调度更新

render阶段从rootFiber开始向下遍历，触发更新的fiber调用 markUpdateLaneFromFiberToRoot 一直向上遍历到rootFiber并返回rootFiber。触发更新的fiber中保存了一个Update的对象。

之后通知Scheduler根据更新的优先级，决定以同步还是异步的方式调度本次更新。

`高优更新`中断正在进行中的`低优更新`，先完成`render - commit流程`。

待`高优更新`完成后，`低优更新`基于`高优更新`的结果`重新更新`。