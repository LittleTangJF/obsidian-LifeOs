一.​**​Store（状态仓库）​**
- **单一数据源​**​
	- 整个应用的状态存储在唯一的 Store 中，通过 `createStore`
	- 分发 Action（`dispatch(action)`）
	- 订阅状态变更（`subscribe(listener)`）
二。**Action（动作）​**
- **描述状态变更​**​：是一个包含 `type`（必选）和 `payload`（可选）的普通 JS 对象
三。​**​Reducer（纯函数）​**
- 接收当前 `state`和 `action`，返回新状态

### 工作流程

- **组件触发 Action​**​：用户交互（如点击按钮）调用 `dispatch(action)`
- ​**​Store 分发 Action​**​：将 Action 传递给 Reducer
-   **​Reducer 计算新状态​**​：根据 Action 类型生成新状态
- **Store 更新状态​**​：替换旧状态，通知所有订阅者。
- **组件重新渲染​**​：连接 Store 的组件通过 `useSelector`获取新状态并更新 UI

### 面试怎么说

**三大原则**
- ​**​单一数据源​**​：整个应用状态存储在单一Store中