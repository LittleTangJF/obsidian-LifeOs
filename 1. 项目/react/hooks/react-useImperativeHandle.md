>自定义ref暴露出来的句柄：就是暴露自定义的方法，暴露你自己命名的方法

- 参考
	- useImperativeHandle(ref, createHandle, dependencies)
- 使用方法
	- 暴露自己命名的方法
	- 不想暴露全部的组件api，暴露少数的定制方法

## 参考

```jsx
forwardRef((props, ref){
		   useImperativeHandle(ref, ()=>{
			   return {
				   // 你的方法
			   }
		   }, [])
}， )
```