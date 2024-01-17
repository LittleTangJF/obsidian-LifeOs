>严格模式 **在开发环境中会调用一些函数两次** 

- 组件函数体 （ 仅限顶层逻辑，不包括事件处理程序内的代码）
- 传递给`useState` `set函数` `useMemo` `useReducer`的函数
- 部分类组件的方法，例如`constructor render  shouldComponentUpdate `等