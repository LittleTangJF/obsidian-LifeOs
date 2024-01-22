## 作用

- 跳过组件的重新渲染
- 从记忆回调中更新state
- 防止频繁触发Effect
- 优化自定义hooks

## 参考

```jsx
useCallback(fn, dependencies)
```

解析：react会在初次渲染时返回该函数，当进行下一次渲染时，如果没有dependencies更新，就返回相同的函数