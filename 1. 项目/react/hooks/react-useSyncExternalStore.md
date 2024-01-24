
> const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot)\

- 用法
	- 订阅外部的store
	- 订阅浏览器API
	- 把逻辑抽离到自定义Hook
	- 添加服务端渲染支持