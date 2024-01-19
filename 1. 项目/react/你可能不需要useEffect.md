> 移除非必要的useEffect让你的代码运行的更快

## 摘要

- 昂贵的计算
	- 使用useMemo
- 在渲染期间处理数据
	- 渲染的值可以用现有的state计算得出
- 想要重置组件树state
	- 可以用id当成组件的key，id变化，组件重新渲染（避免状态未清空问题）
- 事件处理函数
- 链式计算
- 订阅外部的store-->useSyncExternalStore
- 获取数据（return的时候要清除setState）
	- 可以用外部的的hook或者第三方库