>Context上下文允许父组件向其下层无论多深的任何组件传递信息

## 你将会学到

- 什么是‘props’逐级透传
- Context代替方案

```jsx
// 父组件
export const Lcontext = createContext(默认值)
// value是传递的值
<Lcontext.Provider value={level}
	{children}
</ Lcontext.Provider>

// 子组件
const l = useContext(Lcontext)
```

## 特点

- Context会穿透中间层级的组件
- 可读性不是很好，层级不高更推荐props
- 大多数路由组件库用Context来保存当前路由
- 状态管理库也用他来实现状态管理 react-redux