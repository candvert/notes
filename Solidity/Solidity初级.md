源文件可以包含任意数量的 `contract` 定义, `import` 指令, `pragma` 指令和 `using for` 指令 和 `struct`, `enum`, `function`, `error` 以及 `constant` 变量 的定义

每个源文件都应该以一个注释开始，表明其许可证：
```solidity
// SPDX-License-Identifier: MIT
```

pragma 关键字用于启用某些编译器特性或检查

带有该代码的源文件在 0.5.2 版本之前的编译器上不能编译， 在 0.6.0 版本之后的编译器上也不能工作（这第二个条件是通过使用 ^ 添加的）
```solidity
pragma solidity ^0.5.2;
```
## 注释
```solidity
// 单行注释


/*
多行注释
*/
```
## 合约
在 Solidity 中，合约类似于面向对象编程语言中的类。 每个合约中可以包含 状态变量， 函数， 函数修饰器， 事件， 错误， 结构类型 和 枚举类型 的声明，且合约可以从其他合约继承。

还有一些特殊种类的合约，叫做 库合约 和 接口合约。