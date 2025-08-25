### 1.如何判断变量类型

- 基本类型：`typeof`（注意 `typeof null`返回 `object`）
- 复杂类型：`Object.prototype.toString.call()`

### 2.  **​`var`、`let`、`const`的区别？​**

- `var`：函数作用域，存在变量提升
- `let`/`const`：块级作用域，`const`声明不可重新赋值
闭包问题
```js
for (var i = 0; i < 3; i++) { setTimeout(() => console.log(i), 100); // 输出 3,3,3 } 
for (let i = 0; i < 3; i++) { setTimeout(() => console.log(i), 100); // 输出 0,1,2 }
```

### 3.  **​事件循环（Event Loop）执行机制****

- 同步任务 → 微任务队列（Promise.then） → 宏任务队列（setTimeout）

### 4.**手写 Promise.all​**
```js
function promiseAll(promises){
	retrue new promise((resole,reject)=>{
		let resules = [];
		let completed = 0;
		promises.foreach(promise)=>{
			promise.then(res =>{
				resules.push(res)
				if(promises.length === ++completed) resole(resules);
			}.catch(reject)
		}
	}
}
```

### 5. 面向对象与原型

面向对象编程（OOP）是一种​**​以对象为基本单元的编程范式​**​，通过**封装、继承、多态**等机制组织代码

- 原型：在 JavaScript 中，每个函数（构造函数）都有一个 `prototype`属性，指向一个​**​原型对象​**​。原型对象包含共享的属性和方法，所有实例通过 `__proto__`访问该对象
```js
function Person(name) { this.name = name; }  
Person.prototype.sayHello = function() { console.log(`Hello, ${this.name}!`); };  
const alice = new Person("Alice");  
alice.sayHello(); // 调用原型方法
```

- **原型链**：对象通过 `__proto__`链接到其构造函数的原型，若该原型也有原型，则形成链式结构，直至 `Object.prototype`（终点为 `null`）
```js
console.log(alice.__proto__ === Person.prototype); // true  
console.log(Person.prototype.__proto__ === Object.prototype); // true  
console.log(Object.prototype.__proto__); // null
console.log(alice.toString()); // 继承自 Object.prototype
```