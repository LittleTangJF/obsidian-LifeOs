替代类组件的生命周期方法

`useEffect(() => { // 副作用逻辑 return () => { // 清理函数（可选） }; }, [dependencies]); // 依赖数组`

1. ​**​`[]`（空数组）​**
	1. 仅在组件首次挂载时执行一次，卸载时执行清理函数
2. **`[dep1, dep2]`**
	1. 依赖项变化时重新执行副作用，并在下次副作用前清理上一次
3. ​**​清理函数**
	1. 组件卸载时（类似 `componentWillUnmount`）。
	2. 通过 `return () => { ... }`定义，在以下时机执行：
	3. 异步操作中需标记组件状态（如 `isMounted`）避免更新已卸载组件
	`useEffect(() => {`
		  `let isMounted = true;`
		  `fetchData().then(data => {`
		    `if (isMounted) setData(data);`
		  `});`
		  `return () => { isMounted = false; };`
		`}, []);`

## 高级技巧

依赖数组包含**对象/函数**时，每次渲染引用变化触发不必要的副作用。

用 [[react-useMemo]]/[[react-useCallback]]稳定引用，`useMemo`/`useCallback`：记忆复杂计算或函数

用**函数式更新**或检查条件避免无限循环

`useEffect(() => {`
  `if (count < 10) setCount(prev => prev + 1); // 条件终止`
`}, [count]);`