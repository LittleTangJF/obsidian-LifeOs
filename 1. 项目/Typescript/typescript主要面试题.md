### **基础语法与核心概念​**

#### **TypeScript 中 `interface`和 `type`的区别是什么？​**

TypeScript 中 `interface`和 `type`均用于定义类型

​**​差异点​**​：
- `interface`支持声明合并（同名 `interface`自动合并），`type`不支持
- `type`可定义原始类型别名（如 `type Name = string`）、联合类型（`A | B`）、元组（`[string, number]`），而 `interface`只能描述对象
- `interface`可被类 `implements`或 `extends`，`type`需通过交叉类型模拟继承（如 `type Sub = Base & { x: number }`）；
- 性能：`interface`在编译时优化更好（TS 官方推荐优先使用）、
```jsx
  联合类型：`type Status = "success" | "error"`；  
  元组：`type Point = [number, number]`
```

​#### **​`any`和 `unknown`的区别是什么？为什么推荐用 `unknown`替代 `any`？​**

- `any`：关闭类型检查，允许任意操作
- `unknown`：类型安全的“顶级类型”，所有值都是 `unknown`，但操作前需先类型检查或断言（如 `if (typeof a === 'string') { a.length }`）。**强制安全校验**
#### **什么是类型守卫（Type Guard）？常见的类型守卫有哪些？​**

类型守卫是缩小类型范围的表达式
- ​**​`typeof`检查**`if (typeof x === 'string') { ... }`（仅支持 `string`/`number`/`boolean`/`symbol`）；
- ​**​`instanceof`检查**`if (x instanceof Date) { ... }`；
- ​**​`in`操作符**`if ('age' in user) { ... }`（检查对象是否有某属性）。
####   **​泛型（Generic）的作用是什么？如何约束泛型（Generic Constraints）？​**

泛型用于定义可复用的类型无关组件，避免重复代码。约束泛型通过 `extends`关键字限制类型范围。