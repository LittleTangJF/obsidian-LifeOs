
### 0. 数据类型
八种Undefined、Null、Boolean、Number、String、Object、Symbol，BigInt
- 原始数据类型（Undefined、Null、Boolean、Number、String，Symbol，BigInt）
- 引用数据类型（对象、数组和函数）
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

### **原型链与继承​**

子类原型指向父类实例（`Child.prototype = new Parent()`），从而共享父类方法

- ​**​原型链继承​**: 将子类的原型指向父类的实例（`Child.prototype = new Parent()`）但是无法传参
- **构造函数继承（属性）​**：在子类构造函数中调用父类构造函数（`Parent.call(this)`），继承父类实例属性
-   **​组合继承​**：结合原型链继承（方法）和构造函数继承（属性），是 ​**​ES5 最常用方式​**
- **原型式继承​**：​基于已有对象创建新对象（`Object.create()`），无需构造函数
-   **​寄生式继承​**：在原型式继承基础上增强对象，添加新方法
- **寄生组合式继承​**：​**​最优 ES5 继承方案​**​，通过 `Object.create()`复制父类原型，避免调用父类构造函数

```js
function inheritPrototype(Child, Parent) {
    const prototype = Object.create(Parent.prototype); // 复制父类原型
    prototype.constructor = Child;
    Child.prototype = prototype;
}
function Parent(name) { this.name = name; }
function Child(name, age) {
    Parent.call(this, name);
    this.age = age;
}
inheritPrototype(Child, Parent); // 继承方法
const child = new Child('Carol', 30);
```

1.为什么0.1+0.2 ! == 0.3
**浮点数的二进制表示和存储机制​**​ 直接导致
- 0.1的二进制是`0.0001100110011001100...`（1100循环），0.2的二进制是：`0.00110011001100...`（1100循环）
- **计算机只能存储有限位数的二进制​**​（如双精度浮点数保留52位尾数）

**数组去重**

```js
// 方法1：Set 去重 
const unique = [...new Set(arr)];
```

**深拷贝**

```js
function deepClone(obj) { 
	if (obj==null || typeof obj !== 'object') return obj; const clone = Array.isArray(obj) ? [] : {}; 
for (const key in obj) { 
clone[key] = deepClone(obj[key]); } 
return clone; 
}
```