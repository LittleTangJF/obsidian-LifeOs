>在浏览器重新绘制时触发

- 参考
	- useLayoutEffect(setup, dependencies)
- 用法
	- 在浏览器重新绘制屏幕前计算布局

## 用法

1. 一个tooltip，如果上面的高度不够，就显示在下面

```jsx
useLayoutEffect(()=>{
	// 拿到tooltips ref的高度
	setHeight(height)
},[])
```
