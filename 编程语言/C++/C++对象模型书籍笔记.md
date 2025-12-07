```sh
在C++中，有两种类数据成员：static和nonstatic，以及三种类成员函数：static、nonstatic和virtual
```
![](/images/object_model_1.png)
![](/images/object_model_2.png)
## 对齐
```sh
每个成员变量的对齐值由它的类型决定：char为1字节对齐，int为4字节对齐，指针（64位）为8字节对齐
类的总大小为所有成员中最大对齐值的整数倍
成员变量按声明顺序存储
编译器在成员之间插入填充字节，确保每个成员的起始地址是其对齐值的整数倍
```

![](/images/object_model_3.png)

```sh
基类子对象的内存对齐仅由基类的成员决定，与派生类无关
派生类对象的整体对齐值取所有成员（包括基类子对象）的最大对齐值
若基类子对象的末尾地址不满足派生类成员的对齐要求，编译器自动插入填充字节
```

![](/images/object_model_4.png)
