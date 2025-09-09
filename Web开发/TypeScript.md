```typescript
let num: number = 10;
let num = 10 as number;		// 即类型转换
let a: string | null = "abc";
let arr: number[] = [1, 2, 3];
let arr: Array<number> = [1, 2, 3];
let t: [number, string, number] = [1, 'a', 2];	// 元组
let t: [number, string, number?] = [1, 'a'];	// ?代表可选

// typescript的数据类型
string, number, boolean, null, undefined

// 枚举(javascript没有)
enum MyEnum {A, B, C}
console.log(MyEnum.A)
console.log(MyEnum[0])

// 函数
function add(a = 10, b: string, c?: boolean, ...rest: number[]): number {
    return 10
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

// 类型别名
type MyUserName = string | number
let a: MyUserName = 'a'

// 泛型
function myFn<T>(a: T, b: T): T[] {
    return [a, b]
}
myFn<number>(1, 2)
myFn(1, 2)
```

```typescript
// status只能是'pending'或'paid'两者中一个
invoice = {
	status: 'pending' | 'paid'
}
```

```typescript
// 语法分析
// { children }​​       对象解构 (Destructuring)
// Readonly<...>​​       只读泛型 (Generic)​​         表示整个对象是不可变的
// { children: React.ReactNode; }​​  定义对象的形状：必须有一个 children 属性
// React.ReactNode        定义 children 属性的类型
{ children, }: Readonly<{ children: React.ReactNode; }>​​
```
