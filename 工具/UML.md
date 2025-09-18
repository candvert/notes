- [类图](#类图)
	-  [类之间的关系](#类之间的关系)
	- [基数](#基数)
	- [示例](#示例)
- [时序图](#时序图)
	- [示例](#示例)

## UML标准太过复杂，不应追求完全符合标准，而是如何使用UML来更好的沟通交流

### 使用draw.io（免费开源）进行绘制
### 使用PlantUML可以通过简洁的文本生成UML图形
## 类图
```
只有类名是必须的，属性和方法都是可选的

属性和方法前的+、-、#、~的含义：
+ public
- private
# protected
~ package local

函数参数名前可以加in、out或inout（例如下图中的 + study(in course: string): bool）
in表示调用者需设置该参数并传递给函数，而函数不会修改该参数
out表示调用者无需设置该参数，函数会修改该参数
inout表示调用者需设置该参数并传递给函数，并且函数会修改该参数
```
![](images/uml_1.png)
## 类之间的关系
![](images/uml_2.png)
## Inheritance（继承）
![](images/uml_3.png)
## Aggregation（聚合）
![](images/uml_6.png)
## Composition（组合）
![](images/uml_7.png)
## Dependency（依赖）
![](images/uml_8.png)
## Realization（实现）
![](images/uml_4.png)
## 基数
```
0          0个对象
0..1       0个或1个对象
1          必须1个对象
1..1       必须1个对象
0..*       0个或多个对象
*          0个或多个对象
1..*       1个或多个对象
```
![](images/uml_10.png)
![](images/uml_11.png)
## 示例
![](images/uml_15.png)
## 时序图
```
维基百科上的定义：A sequence diagram shows, as parallel vertical lines (lifelines), different processes or objects that live simultaneously, and, as horizontal arrows, the messages exchanged between them in the order in which they occur.

水平方向表示涉及到的对象
垂直方向表示时间

消息以水平箭头书写，上方写有消息名称，用于显示交互。实心箭头表示同步调用，空箭头表示异步消息，虚线表示回复消息。如果调用者发送同步消息，则必须等到消息完成，例如调用子程序。如果调用者发送异步消息，它可以继续处理而不必等待响应。如果对象被销毁（从内存中删除），生命线下方会画一个 X，虚线不再画在生命线下方。

alt         只有条件为真的片段才会执行
opt         仅当提供的条件为真时，该片段才会执行
loop        该片段可能执行多次
region      该片段一次只能有一个线程执行它
neg         该片段显示了无效的交互

```
## 示例
![](images/uml_9.png)
![](images/uml_12.png)
![](images/uml_13.png)
