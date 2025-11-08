## 测试
测试文件通常以 .test.ts 结尾，并通常放在源码所在目录下的 tests 文件夹中
## other
具有类型推断功能
支持函数重载（JavaScript不支持）
```typescript
// 类型注解
let num: number;
let num: number = 10;
// 类型断言
let numArr = [1, 2, 3]
const result = numArr.find(item => item > 2) as number
result * 5    // 如果上句没有as number，该句会报错，因为result可能是undefined




let a: 1 | 2 | 5 = 2;
let a: string | null = "abc";
// 数组类型的两种表示方法
let arr: number[] = [1, 2, 3];
let arr: Array<number> = [1, 2, 3];
// 元组
let t: [number, string, number] = [1, 'a', 2];
// ?代表可选
let t: [number, string, number?] = [1, 'a'];






// typescript的数据类型
string, number, boolean, null, undefined






// 枚举(javascript没有)
enum MyEnum {A, B, C}
// 两种访问方式
console.log(MyEnum.A)
console.log(MyEnum[0])






// 函数
// 所有c?: boolean这样的可选参数要放在右侧
function add(a = 10, b: string, c?: boolean, ...rest: number[]): number {
    return 10
}
// 可以用void表示没有返回值
function no(): void {
    10 + 2;
}






// 接口
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






// 定义新类型
type MyUserName = string | number
type State = {
  errors?: {
    email?: string[];
  };
  message?: string | null;
};
let a: MyUserName = 'a'






// 泛型
function myFn<T>(a: T, b: T): T[] {
    return [a, b]
}
// 使用
myFn<number>(1, 2)
myFn(1, 2)
```

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