简介：分为函数组件和类组件
### 函数组件

`const Welcome = (props) => <h1>Hello, {props.name}</h1>;`
### 类组件
含有生命周期 [[类组件生命周期]]
``
`class Clock extends React.Component {`
  `componentDidMount() { /* 初始化操作 */ }`
  `render() { return <div>{this.state.time}</div>; }`
`}`
`


- \<StrictMode>: [[react-严格模式]]
	- 严格模式会渲染组件两次，用于检查react组件在给定props state context时必须返回相同的jsx，违反此规则会报错
- \<Profiler> : [[react-测量渲染性能]]
	- 性能分析会增加额外开销，在默认情况下，生产模式下是被禁用的
- \<Suspense>: [[react-内容加载时的渲染组件]]
	- 用于当内容正在加载时显示后备方案