### **基础语法与核心概念​**

==TypeScript 的基础语法在 JavaScript 的基础上增加了​**​静态类型系统**==核心价值是==静态类型检查提升代码健壮性==

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

​####  `any`和 `unknown`的区别是什么？为什么推荐用 `unknown`替代 `any`？​

- `any`：关闭类型检查，允许任意操作
- `unknown`：类型安全的“顶级类型”，所有值都是 `unknown`，但操作前需先类型检查或断言（如 `if (typeof a === 'string') { a.length }`）。**强制安全校验**
#### **什么是类型守卫（Type Guard）？常见的类型守卫有哪些？​**

类型守卫是缩小类型范围的表达式
- ​**​`typeof`检查**`if (typeof x === 'string') { ... }`（仅支持 `string`/`number`/`boolean`/`symbol`）；
- ​**​`instanceof`检查**`if (x instanceof Date) { ... }`；
- ​**​`in`操作符**`if ('age' in user) { ... }`（检查对象是否有某属性）。
####   **​泛型（Generic）的作用是什么？如何约束泛型（Generic Constraints）？​**

泛型用于定义可复用的类型无关组件，避免**重复代码**。约束泛型通过 `extends`关键字限制类型范围。

- **代码复用**：通过类型参数化，使同一段逻辑能处理多种数据类型

  
​**​条件类型（Conditional Types）和映射类型（Mapped Types）的区别是什么？​**

- **条件类型​**​：根据条件选择类型（类似三元表达式），语法 `T extends U ? X : Y`；
- **映射类型​**​：遍历已有类型的属性，生成新类型，语法 `[K in keyof T]: ...`。

  
​**​TS 内置的工具类型（Utility Types）有哪些？举例说明用途。​**

- `Partial<T>`：所有属性变为可选（`{ [K in keyof T]?: T[K] }`）；
- `Required<T>`：所有属性变为必选（`{ [K in keyof T]-?: T[K] }`）；
- `Pick<T, K>`：从 `T`中选取 `K`指定的属性（`{ [P in K]: T[P] }`）；
- `Omit<T, K>`：从 `T`中排除 `K`指定的属性（`Pick<T, Exclude<keyof T, K>>`）；
- `Readonly<T>`：所有属性变为只读（`{ readonly [K in keyof T]: T[K] }`）；
- `Record<K, V>`：定义键为 `K`、值为 `V`的对象（`{ [P in K]: V }`）。

**类型断言（`as`）和类型守卫的区别是什么？何时用哪个？​**

- ​**​类型断言​**​：告诉编译器“我知道这个值的类型”（绕过类型检查），可能导致运行时错误（如 `const a = someValue as string`）；
- ​**​类型守卫​**​：通过代码逻辑（如 `typeof`/`instanceof`）证明类型，编译器自动缩小类型范围（安全）。
能用类型守卫（安全）就不用断言（危险），仅在明确知道类型且无法用守卫时使用断言（如操作 DOM 元素 `el as HTMLInputElement`）。

​**​TS 中的 `never`类型有什么作用？​**

- `ever`表示永远不会出现的类型（如抛出错误的函数、无限循环）

**​TS 中的 `inter`关键字有什么作用？​**

TypeScript 中的 `infer`关键字是​**​条件类型（Conditional Types）​**​的核心功能，用于在类型匹配过程中​**​动态推断并提取嵌套的类型信息​**
- 它必须与 `extends`结合使用​