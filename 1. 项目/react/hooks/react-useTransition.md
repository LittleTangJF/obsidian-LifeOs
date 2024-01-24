
>帮助你不阻塞UI的情况下更新状态的Hook

- 概念
	- const [ispending, startTransition] = useTransition()
	- ispending：告诉你是否存在待处理的transition
	- startTransition函数：你可以使用此方法将状态标记为transition
		- 参数：一个通过调用一个或者多个set函数更新状态的函数
			- 必须是同步的函数
- 用法
	- 将状态更新标记为非阻塞的transition
	- 




## 将状态更新标记为非阻塞的transition

- 比如有三个选项卡，中间的一个比较缓慢，正常先点击中间，然后点击右边后，UI会变得无响应，卡一会才成功切换 #阻止用户交互

所以，可以使用startTransition函数去将set函数标记为transition状态，然后能正常无卡顿响应

```jsx
function selectTab(next){
	startTransition(()=>{
		setTab(next)
	})
}
```

