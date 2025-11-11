- [数据类型](#数据类型)
- [联合类型](#联合类型)
- [类型注解](#类型注解)
- [类型断言](#类型断言)
- [数组](#数组)
- [元组](#元组)
- [枚举(javascript没有)](#枚举(javascript没有))
- [函数](#函数)
- [接口](#接口)
- [定义新类型](#定义新类型)
- [泛型](#泛型)
- [类型分析示例](#类型分析示例)
- [satisfies关键字](#satisfies关键字)
- [测试](#测试)

- [??空值合并操作符](#??空值合并操作符)
- [?.可选链操作符](#?.可选链操作符)
- [!非空断言操作符](#!非空断言操作符)

支持函数重载（JavaScript不支持）
## 数据类型
```ts
// string, number, boolean, null, undefined, void, symbol, bigint, any
// any 会跳过所有类型检查，unknown 需先断言或类型收窄才能使用
// void 表示函数无返回值（返回 undefined），never 表示函数永远不返回（抛出异常或死循环）

// 字符串
let name: string = "Alice";
// 数字（包括整数、浮点数、二进制等）
let age: number = 30;
let price: number = 9.99;
// 布尔值
let isActive: boolean = true;
// 空值
let empty: void = undefined;    // 通常用于函数无返回值
function warn(): void { console.log("Warning"); }
// Null 和 Undefined
let u: undefined = undefined;
let n: null = null;
// Symbol（ES6 唯一标识）
const key: symbol = Symbol("unique");
// BigInt（大整数）
const big: bigint = 9007199254740991n;


// 任意类型（关闭类型检查）
let anything: any = "could be anything";
anything = 42;  // 允许重新赋值任意类型

// 未知类型（类型安全的 any）
let uncertain: unknown = "hello";
uncertain.toFixed(2);  // 错误：需先类型检查
if (typeof uncertain === "number") {
  uncertain.toFixed(2);  // 正确
}

// 元组（固定长度和类型）
let person: [string, number] = ["Alice", 30];
// 枚举
enum Color { Red, Green, Blue }
let c: Color = Color.Green;  // 值为 1
// 永不存在的值
function error(message: string): never {
  throw new Error(message);
}
```
## 联合类型
```ts
// 联合类型使用竖线符号 | 来连接多个类型，表示一个值可以是这些类型中的任意一种
let a: 1 | 2 | 5 = 2;
let a: string | null = "abc";
```
## 类型注解
```typescript
let num: number;
let num: number = 10;
```
## 类型断言
```ts
let numArr = [1, 2, 3]
const result = numArr.find(item => item > 2) as number
result * 5    // 如果上句没有as number，该句会报错，因为result可能是undefined
```
## 数组
```ts
// 数组类型的两种表示方法
let arr: number[] = [1, 2, 3];
let arr: Array<number> = [1, 2, 3];
```
## 元组
```ts
let t: [number, string, number] = [1, 'a', 2];
// ?代表可选
let t: [number, string, number?] = [1, 'a'];
```
## 枚举(javascript没有)
```ts
enum MyEnum {A, B, C}
// 两种访问方式
console.log(MyEnum.A)
console.log(MyEnum[0])
```
## 函数
```ts
// 
// 所有c?: boolean这样的可选参数要放在右侧
function add(a = 10, b: string, c?: boolean, ...rest: number[]): number {
    return 10
}
// 可以用void表示没有返回值
function no(): void {
    10 + 2;
}
```
## 接口
```ts
interface Obj {
    name: string,
    age: number
}
const obj: Obj = {
    name: 'a',
    age: 10
}
// 接口继承
interface Parent {
	prop1: string,
	prop2: number
}
interface Child extends Parent {
	prop3: string
}
const myObj: Child = {
	prop1: '',
	prop2: 1,
	prop3: ''
}
```
## 定义新类型
```ts
type MyUserName = string | number
type State = {
  errors?: {
    email?: string[];
  };
  message?: string | null;
};
let a: MyUserName = 'a'
```
## 泛型
```ts
function myFn<T>(a: T, b: T): T[] {
    return [a, b]
}
// 使用
myFn<number>(1, 2)
myFn<number | string | null>(1, 2)
myFn(1, 2)
```
## 类型分析示例
```typescript
// 语法分析
// { children }​​       对象解构 (Destructuring)
// Readonly<...>​​       只读泛型 (Generic)​​         表示整个对象是不可变的
// { children: React.ReactNode; }​​  定义对象的形状：必须有一个 children 属性
// React.ReactNode        定义 children 属性的类型
{ children, }: Readonly<{ children: React.ReactNode; }>​​
```
## satisfies关键字
```ts
// 确保你定义的对象（components）满足（satisfies）MDXComponents接口的要求。如果对象缺少必需的属性或者属性类型不匹配，TypeScript 编译器会报错
const components = {} satisfies MDXComponents
```
## 测试
测试文件通常以 .test.ts 结尾，并通常放在源码所在目录下的 tests 文件夹中
## ??空值合并操作符
```ts
let a: string | null = "abc";
let length = a?.length ?? 0; // 如果 a 为 null，length 为 0
console.log(length); // 输出：3
```
## ?.可选链操作符
```ts
let a: string | null = "abc";
console.log(a?.length); // 输出：3（如果 a 为 null，则输出 undefined）
```
## !非空断言操作符
```ts
// 非空断言操作符 ! 的主要作用是告诉 TypeScript 编译器：你确信某个表达式的结果一定不为 null 或 undefined，即使编译器的类型检查无法自动推断出这一点
```