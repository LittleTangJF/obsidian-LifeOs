
> 让你订阅外部的store

- 使用
- const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot)
	- subscribe函数：订阅和返回取消订阅的函数
	- getSnapshot函数：返回store数据的快照
- 用法
	- 订阅外部的store
	- 订阅浏览器API
	- 把逻辑抽离到自定义Hook
	- 添加服务端渲染支持

## 订阅外部的store

比如订阅一个todos的外部store

## 订阅浏览器API

比如订阅浏览器的API navigator.onLine，来展示网络是否正常

```jsx
function subscribe(){
	windows.addEventListenner('online')
	return ()=>{
	windows.removeEventListenner('online')
	}
}

function getSnapshot(){
	return navigator.online
}
```

所以，可以把上诉逻逻辑抽离到自定义Hook

