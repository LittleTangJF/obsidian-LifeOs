1.**​React性能优化的10种方法**

- 使用[[react.memo]]避免不必要的组件渲染
- 通过[[react-useMemo]]缓存计算结果
- [[路由懒加载]]（React.lazy + Suspense）
- 减少内联函数定义（使用[[react-useCallback]]）
- 虚拟化长列表（react-window）第三方包
- 服务端渲染（SSR）首屏加载优化
- 代码分割（Dynamic Import）
- 使用React 18的并发模式优化交互-可中断、lane优先级