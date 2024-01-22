>Context上下文允许父组件向其下层无论多深的任何组件传递信息

## 你将会学到

- 什么是‘props’逐级透传
- Context代替方案

```jsx
// 父组件
export const Lcontext = createContext(默认值)
// 
<Lcontext.Provider value={level}
	{children}
</ Lcontext.Provider>

// 子组件
const l = useContext(Lcontext)
```