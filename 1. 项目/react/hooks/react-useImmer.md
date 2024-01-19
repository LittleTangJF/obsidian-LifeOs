>react中state是只读，想要修改state可以通过useImmer来写命令式的代码

```jsx
const [todo, updateTodo] = useImmer(initial)

function change(){
	updateTodo(draft=>{
		draft.push({
			id: 1,
			name: 'haha'
		})
	})
}
```