- [类图](#类图)
	-  [类之间的关系](#类之间的关系)
	- [示例](#示例)
### UML标准太过复杂，不应追求完全符合标准，而是如何使用UML来更好的沟通交流

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
## 示例
![](images/uml_15.png)
## 时序图
## 示例
![](images/uml_9.png)
