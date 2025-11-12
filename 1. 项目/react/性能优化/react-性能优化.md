1.**​React性能优化的10种方法**

- 使用[[react.memo]]避免不必要的组件渲染
- 通过[[react-useMemo]]缓存计算结果
- [[路由懒加载]]（React.lazy + Suspense）
- 减少内联函数定义（使用[[react-useCallback]]）
- 虚拟化长列表（react-window）第三方包
- 服务端渲染（SSR）首屏加载优化
- 代码分割（Dynamic Import）
- 使用React 18的并发模式优化交互-可中断、lane优先级
	- import { createRoot } from "react-dom/client";createRoot(document.getElementById("root")).render(<App />);
- 减少回流和重绘：不要频繁操作元素的样式
	- 回流：页面首次渲染、窗口大小变化、文字大小
	- 重绘：样式发生变化但不影响布局：color、background、border-radius
- 节流与防抖
	- 防抖：指在事件被触发 n 秒后再执行回调，如果在这 n 秒内事件又被触发，则重新计时
	- 节流：规定一个单位时间，在这个单位时间内，只能有一次触发事件