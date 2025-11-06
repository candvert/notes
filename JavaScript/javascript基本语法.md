- [javascript特点](#javascript特点)
- [基础](#基础)
	- [编写地方](#编写地方)
	- [输出](#输出)
	- [变量](#变量)
	- [常量](#常量)
	- [注释](#注释)
	- [字符串](#字符串)
	- [数据类型](#数据类型)
	- [类型转换](#类型转换)
	- [数组](#数组)
	- [相等运算符](#相等运算符)
	- [条件语句](#条件语句)
	- [循环](#循环)
	- [模块](#模块)
	- [export](#export)
	- [import](#import)
- [函数](#函数)
- [对象](#对象)
	- [原型和原型链](#原型和原型链)
- [类](#类)
- [异常](#异常)
- [异步](#异步)
- [fetch](#fetch)
- [其他知识点](#其他知识点)
	- [解构赋值的重命名](#解构赋值的重命名)
	- [...扩展运算符](#...扩展运算符)
	- [?.](#?.)
	- [??空值合并运算符](#??空值合并运算符)
	- [处理json](#处理json)

关于异步、worker等不熟悉的主题可以在[mozilla](https://developer.mozilla.org/zh-CN/)上学习
## javascript特点
```js
// js 有垃圾回收

// js 是动态类型语言

// js 中除了七种基本数据类型（也称为原始类型）外，其他所有值都是对象

// js 中没有运算符重载，没有宏，没有函数和方法重载

// 如果一条语句独占一行，则分号可以省略。但是好的做法是每条语句末尾都加上分号

// js 有三元运算符 1 < 2 ? 'hello' : 'world'
```
## 基础
## 编写地方
```html
<!--在 html 文件中直接编写 js 语句，下面这句通常放在 </body> 标签前-->
<script>
	// 在此编写 JavaScript 代码
</script>


<!--单独编写一个 js 文件，再通过下面语句引入到 html 文件中，下面这句通常放在 <head> 标签中-->
<!--需要注意：浏览器限制在 file:// 协议下加载 ES 模块，ES 模块设计为在 HTTP 服务器环境下工作，无法在本地文件协议下正常加载-->
<script type="module" src="script.js"></script>
```
## 运行单个文件
```js
node a.js
```
## 输出
```js
console.log("ok");
```
## 变量
```js
// 变量声明，当变量被声明但未被赋值时，其值就是 undefined
let num;
// js 是动态类型语言，可以重新赋值为不同类型的值
num = 10;
num = "hello"


// 声明并初始化多个变量
let name = 'hello', age = 20;




// var 是遗留的，不建议使用
var grade = 10;
// 可以在初始化一个变量之后用 var 声明它，它仍然可以工作，但不推荐使用
age = 18;
var age;
```
## 常量
```js
const num = 78;
```
## 注释
```js
// 单行注释


/*
多行注释
*/
```
## 字符串
```js
// js 中没有所谓的 char 类型，单个字符被视为长度为 1 的字符串
// 单引号和双引号都可以
const s1 = 'hello';
const s2 = "hello";



const name = "John";
// 模板字符串，用反引号包裹，可以在 ${ } 中放入 JavaScript 变量或表达式
const greeting = `hello, ${name}`;


// 使用 + 运算符连接普通字符串
console.log(greeting + "，" + name);


// 多行字符串，模板字符串会保留源代码中的换行符
s = `hello
	world`;


// 连接字符串和数字
console.log("top " + 3); // "top 3"
console.log(3 + "apple"); // "3 apple"
```
## 数据类型
```js
// 七种基本数据类型：Boolean、null、undefined、Number、BigInt、String、Symbol
// 当变量被声明但未被赋值时，其值就是 undefined


// typeof 返回变量的类型
const name = "hi";
console.log(typeof name);
```
## 类型转换
```js
// 显式类型转换
const num = "10";
const n = Number(num);
const s = String(n);
```
## 数组
```js
// 数组可以包含不同类型的元素
const arr = ["tree", 795, [0, 1, 2]];
arr[1] = "food";
arr[2][0] = 3;

// 空数组
const l = [];


// 数组的长度
console.log(arr.length);


// 添加元素
arr.push("ok");
arr.push("hello", "world");
// 添加到数组开头
arr.unshift("ok");


// 删除元素
arr.pop();
// 删除第一个元素
arr.shift();
```
## 相等运算符
```js
// 结果为 true，== 只测试值是否相同，而不判断类型是否相同
console.log(2 == '2');


// 结果为 false，进行比较时应该使用 === 和 !==
console.log(2 === '2');
```
## 条件语句
```js
// if 语句，switch 语句和 c++ 相同


// if 语句
const num = 16;
if (num > 10) {
	console.log("hello");
} else if (num > 20) {
	console.log("hello");
} else {
	console.log("hello");
}


// switch 语句
const age = 18
switch (age) {
	case 18:
		console.log("hello");
		break;
	default:
		console.log("hello");
}
```
## 循环
```js
// for 语句，while 语句，do...while 语句和 c++ 相同


// for 语句
for (let i = 0; i < 10; i++) {
	if (i === 5) continue;
	if (i === 8) break;
	console.log(i);
}


// while 语句
let i = 5;
while (i > 0) {
	console.log(i);
	i--;
}


// do...while 语句
let i = 5;
do {
	console.log(i);
	i--;
} while (i > 0);


// 遍历可迭代对象
let nums = [23, 35, 67];
for (num of nums) {
	console.log(num);
}


// 遍历对象的属性
let Person = {
    name: 'John',
    age: 20,
}
for (key in Person) {
	console.log(Person[key]);
}
```
## 模块
```js
// 每个 js 文件就是一个模块
```
## export
```js
// 默认导出
// 每个模块只能有一个默认导出
const message = "Hello World";
export default message;




// 导出名字
// 方式 1: 声明时直接导出
export const name = "Bill";
export function greet() {
    console.log("Hello!");
}


// 方式 2: 在文件底部统一导出
const age = 18;
function farewell() {
    console.log("goodbye");
}
export { age, farewell };
```
## import
```js
// 导入名字
import { name, age } from './person.js';

// 导入默认导出的名字
import myMessage from './message.js';

// 混合导入
import defaultExport, { namedExport1, namedExport2 } from './module.js';

// 别名导入
import { namedExport as newName } from './module.js';

// 导入所有名字
import * as utils from './utils.js';
```
## 函数
```js
function eat(a, b = 0, c) {
    console.log(a, b, c);
}
// 若对应位置的实参未提供，则值为 undefined
eat();               // undefined 0 undefined
eat(2, 3, 4);        // 2 3 4
// 多余的实参会被忽略
eat(2, 3, 4, 5);     // 2 3 4



// 匿名函数（arrow function）
// 没有参数
let f1 = () => console.log("hi");
// 一个参数
let f2 = num => console.log(num);
// 多个参数
let f3 = (a, b) => { console.log(a, b); };
f1();
f2(5);
f3(5, 6);



// 匿名函数，不推荐使用，推荐使用 arrow function
let f1 = function() { console.log("hi") };
let f2 = ( function() { console.log("hi") } );
f1();
f2();
// 直接调用
( function() { console.log("hi") } )();
( function() { console.log("hi") }() );
```
## 对象
```js
// 创建一个空对象
const Animal = {};


// 创建一个对象
const Person = {
	name: "Bob",
}
// 更新属性
Person.name = "John";
// 也可以以这种方式访问，但 [] 内的值必须为字符串
Person["name"] = "John";
// 添加新的属性
Person.age = 18;
// 添加新的方法
Person.farewell = () => { console.log("goodbye"); };


// 对象可以嵌套
const Person2 = {
	name: {
		first: "Bob",
		second: "Smith",
	},
}
Person2.name.first = "John";


// this 关键字
const Pet = {
	name: "dog",
	introduce() {
		console.log(`I'm ${this.name}`);
	},
	// 也可以这样定义方法
	ok: function() {
		console.log("hi");
	},
}
Pet.ok();



// 构造函数，更推荐使用类
function Teacher(name) {
	this.name = name;
	this.introduce = function () {
		console.log(`I'm ${this.name}`);
	};
}
// 创建对象
const salva = new Teacher("Salva");
salva.introduce();
```
## 原型和原型链
```js
// js 中的每个对象都有一个内置属性，称为其原型。原型本身就是一个对象，因此原型将拥有自己的原型，形成所谓的原型链，直到原型是 null 的对象。根据定义，null 没有原型
// 当试图访问对象的属性时，不仅在该对象上查找属性，还会在该对象的原型上查找属性，以及原型的原型，依此类推，直到找到一个名字匹配的属性或到达原型链的末尾
// 在运行时修改原型链的任何成员、甚至是换掉原型都是可能的


// 访问对象的原型
const obj = {};
const proto = Object.getPrototypeOf(obj);
console.log(proto === Object.prototype); // true



// this 所指的对象
const parent = {
	value: 2,
	method() {
		return this.value + 1;
	},
};
console.log(parent.method()); // 3
// 当调用 parent.method 时，“this”指向了 parent

// child 是一个继承了 parent 的对象
const child = {
  __proto__: parent,
};
console.log(child.method()); // 3
// 调用 child.method 时，“this”指向了 child。
// 又因为 child 继承的是 parent 的方法，
// 首先在 child 上寻找属性“value”。
// 然而，因为 child 没有名为“value”的自有属性，
// 该属性会在 [[Prototype]] 上被找到，即 parent.value。

child.value = 4; // 将 child 上的属性“value”赋值为 4。
// 这会遮蔽 parent 上的“value”属性。
// child 对象现在看起来是这样的：
// { value: 4, __proto__: { value: 2, method: [Function] } }
console.log(child.method()); // 5
// 因为 child 现在拥有“value”属性，“this.value”现在表示 child.value
```
## 类
```js
// js 中的 class 本质上是一个语法糖，底层仍是 js 的原型链机制
// js 是单继承模型，没有多继承



// 空类
class Student {}


class Student {
	name;
	age = 18;
	say() {}
}
const s = new Student();
console.log(s.age);



class Student {
	// 构造函数
	constructor(name, age) {
		this.name = name;
		this.age = age;
	}

	// 普通方法
	introduce() {
		console.log(`I'm ${this.name}`);
	}

	// 静态方法
	static say() {
		console.log("hi");
	}

	// 静态属性
	static grade = 98;
}
const stu = new Student('Alice', 30);
stu.introduce();
Student.say();
console.log(Student.grade);
// 静态属性和方法不能通过实例访问
stu.say(); // 报错
console.log(Student.grade); // 报错




// Getter 和 Setter
class Animal {
	constructor(age) {
		this._age = age; // 约定俗成，内部属性前加下划线
	}
	
	get age() {
		return this._age;
	}
	
	set age(value) {
		this._age = value;
	}

	// 也可以这样使用 Getter
	get weight() {
		return 16;
	}
}
const a = new Animal(18);
a.age = 20;
console.log(a.age);
// _age 也是可以直接访问的
console.log(a._age);
console.log(a.weight);



// ES2022 正式引入了以 # 前缀标识的原生私有字段语法
class Person {
    #name; // 声明私有属性
    #age = 0;

    constructor(name, age) {
        this.#name = name;
        this.#age = age;
    }

    // 私有方法
    #validateAge(age) {
        return age >= 0 && age <= 120;
    }

	// 静态私有方法
	static #say() {
		console.log("hi");
	}

    // 静态私有属性
    static #maxAge = 120;
}
const person = new Person('David', 25);
console.log(person.#name); // SyntaxError: 私有字段必须在声明类中访问




// js 是单继承模型，没有多继承
// 父类的使用 # 前缀定义的私有字段，子类是无法直接访问的
class Animal {
	constructor(name) {
		this.name = name;
	}
	speak() {
		console.log(`${this.name} makes a noise.`);
	}
}
class Dog extends Animal {
	constructor(name, breed) {
		// 调用父类的构造函数
		super(name);
		this.breed = breed; // 子类自己的属性
	}
	
	speak() {
		super.speak(); // 先调用父类的 speak 方法
		console.log(`${this.name} barks.`); // 然后扩展子类自己的行为
	}
}
```
## 异常
```js
// finally 块是可选的：无论是否发生异常，该块中的代码都会执行。常用于清理资源，如关闭文件或网络连接


// 抛出异常
throw new Error("输入值不能为空");


try {
	nonExistentFunction(); // 这是一个不存在的函数，会抛出 ReferenceError
} catch (error) {
	console.error("出错了：", error.message);
} finally {
	console.log("这部分代码总是会执行。");
}
```
## 异步
```js
// 回调函数，不过容易形成“回调地狱”
function fetchData(callback) {
	setTimeout(() => {
		callback('Data received');
	}, 1000);
}
fetchData((data) => {
	console.log(data);
});



// Promise
// 本质上 Promise 是一个函数返回的对象，我们可以在它上面绑定回调函数
function doSomething() {
	return new Promise((resolve) => {
		setTimeout(() => {
			// promise 的兑现值
			resolve("hello");
		}, 200);
	});
}
// 成功时调用 then 传递进来的函数，失败时调用 catch 传递进来的函数
doSomething()
	.then(data => console.log(data)) // "hello"
	.catch(error => console.error(error));

// 链式调用
fetch('https://api.example.com/data')
	.then(response => response.json())
	.then(json => console.log(json))
	.catch(error => console.error('Error:', error))
	// 无论是否成功，finally 传递的函数都会被调用
	.finally(() => { /* ... */ });



// async 和 await，它们是基于 Promise 的语法糖，进一步提升了代码的可读性
// await 关键字必须在 async 函数内部使用
// 一个使用 async 关键字声明的函数，无论其内部是否包含 await，它都会返回一个 Promise 对象
// 在模块的顶层作用域中，可以直接使用 await，而无需包裹在 async 函数内。这个特性被称为顶层 await
async function fetchDataAsync() {
	try {
		const response = await fetch('https://api.example.com/data');
		const data = await response.json();
		console.log(data);
	} catch (error) {
		console.error('Request Failed', error);
	}
}
fetchDataAsync();
```
## fetch
```js
// GET 请求
const response = await fetch('/');
// response.json() 返回的是一个对象
const data = await response.json();
// 访问 json 中 person 键对应的值
console.log(data.person);



// POST 请求
const response = await fetch("/api/chat", {
  method: "POST",
  // 发送 json 数据
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "John" }),
  // 发送纯文本
  // headers: { "Content-Type": "text/plain" },
  // body: "hello",
});
if (!response.ok) {
  throw new Error(`HTTP error! status: ${response.status}`);
}
const data = await response.json(); 
console.log(data.person);



// 如果连接是类型是 "Content-Type": "text/event-stream"，即流式传输，则这样处理
const reader = response.body.getReader();
const decoder = new TextDecoder('utf-8');
while (true) {
  const { done, value } = await reader.read();
  if (done) break;
  // 使用 { stream: true } 表示数据流还未结束
  const chunk = decoder.decode(value, { stream: true });
  // Server-Sent Events (SSE) 流式响应协议规定，服务器发送的每条完整消息必须以两个连续的换行符（即 \n\n）作为结束标志
  const lines = chunk.split("\n\n");
  for (const line of lines) {
    if (line.startsWith('data: ')) {
      try {
	    const data = JSON.parse(line.slice(6));
		// JSON.parse() 返回的是一个对象
	    console.log(data.person);
	  } catch (e) {
	    console.error('解析JSON失败:', e, '，原始消息:', message);
	  }
    }
  }
}




// fetch() 函数返回的 Response 对象具有的属性
.ok // true 或 false
.status // 状态码
.url // url
.text() // 返回 USVString 格式的 Promise 对象
.json() // 返回 JSON 格式的 Promise 对象
```
## 其他知识点
## 解构赋值的重命名
```js
// data: metadata的意思是将data的值赋给metadata，两个变量都能使用
let { data: metadata, content } = readMDXFile();
console.log(metadata);
```
## ...扩展运算符
```js
// ...是扩展运算符，用于将可迭代对象（如数组或字符串）展开为单个元素
// 合并数组
const arr1 = [1, 2];
const arr2 = [3, 4];
const mergedArr = [...arr1, ...arr2]; // [1, 2, 3, 4]

// 扩展运算符进行的是​​浅拷贝
const originalObj = { a: 1, b: 2 };
const copyObj = { ...originalObj }; // { a: 1, b: 2 }
```
## ?.
```js
// ?. 在 JavaScript 中如果调用对象是 null，则返回 undefined
// 假设我们有一个用户对象，但不确定某些信息是否存在
const user = {
  name: 'Alice',
  address: {
    city: 'Beijing'
  },
  sayHello: function() {
    return `Hello, ${this.name}!`;
  }
};

// 安全访问可能不存在的属性
console.log(user?.address?.city); // 输出 "Beijing"
console.log(user?.contact?.phone); // 输出 undefined，而不会报错

// 安全调用可能不存在的方法
console.log(user.sayHello?.()); // 输出 "Hello, Alice!"
console.log(user.nonExistentMethod?.()); // 输出 undefined，而不会报错

// 结合空值合并运算符提供默认值
const phoneNumber = user?.contact?.phone ?? '暂无电话';
console.log(phoneNumber); // 输出 "暂无电话"

// 访问数组元素
const firstItem = someArray?.[0]; // 如果 someArray 为 null 或 undefined，则返回 undefined
```
## ??空值合并运算符
```js
// ??是空值合并运算符（Nullish Coalescing Operator）。它的作用是返回其左侧操作数，如果左侧操作数为 null 或 undefined，则返回右侧操作数
const nullValue = null;
const undefinedValue = undefined;
const zero = 0;
const emptyString = '';
const normalText = 'Hello';

console.log(nullValue ?? 'default');        // 输出: 'default'
console.log(undefinedValue ?? 'default');  // 输出: 'default'
console.log(zero ?? 42);                   // 输出: 0 (保留了 0)
console.log(emptyString ?? 'default');     // 输出: '' (保留了空字符串)
console.log(normalText ?? 'default');      // 输出: 'Hello'
```
## 处理json
```js
// 只包含属性，不包含方法
// JSON 要求在字符串和属性名称周围使用双引号。除了围绕整个 JSON 字符串之外，单引号无效
// JSON 实际上可以采用任何可以包含在 JSON 中的数据类型，而不仅仅是数组或对象。例如，单个字符串或数字就是有效的 JSON
JSON.parse()		// 接受 JSON 字符串作为参数，并返回相应的 JavaScript 对象
JSON.stringify()	// 接受一个对象作为参数，并返回等效的 JSON 字符串
```
