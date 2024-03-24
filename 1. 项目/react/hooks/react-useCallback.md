## 作用

- 跳过组件的重新渲染：传递给子组件的函数（**搭配memo组件**）
- 从记忆回调中更新state
- 防止频繁触发Effect
- 优化自定义hooks

>需要写入依赖，不然每次都会重新计算

## 参考

```jsx
useCallback(fn, dependencies)
```

解析：react会在初次渲染时返回该函数，当进行下一次渲染时，如果没有dependencies更新，就返回相同的函数

>缓存该函数，保持引用不变，dependencies更新后才改变其引用

>#默认情况下，当一个组件重新渲染时，React会递归渲染它的所有子组件