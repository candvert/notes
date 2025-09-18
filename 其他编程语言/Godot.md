- [GDScript](#GDScript)
	- [输出](#输出)
	- [访问子节点](#访问子节点)
	- [节点的函数](#节点的函数)
	- [input函数（当有按键按下等会调用）](#input函数（当有按键按下等会调用）)
	- [变量](#变量)
	- [get和set](#get和set)
	- [await](#await)
	- [数据类型和类型转换](#数据类型和类型转换)
	- [数组](#数组)
	- [字典](#字典)
	- [枚举](#枚举)
	- [随机数](#随机数)
	- [条件语句](#条件语句)
	- [循环](#循环)
	- [信号](#信号)
	- [函数](#函数)
	- [继承](#继承)
	- [inner class](#inner class)
- [Godot](#Godot)
	- [人物scene](#人物scene)
	- [游戏scene](#游戏scene)
	- [移动平台scene](#移动平台scene)
	- [Vector2](#Vector2)
	- [Input](#Input)
	- [Node](#Node)
	- [CharacterBody2D](#CharacterBody2D)
	- [文件](#文件)
	- [Json](#Json)

# GDScript
## 输出
```python
print("hello world")
```
## 访问子节点
```python
# 比如下列文件结构
# Node2D
# 	Label
# 		Weapon
# 则在Node2D中访问子节点为
$Label
$Label.text = "hello"
$Label/Weapon
$Label/Weapon.text = "sword"

# @onready表示等子节点加载完才可用
@onready var weapon = $Label/Weapon
# $是get_node函数的缩写
@onready var weapon = get_node("Label/Weapon")
```
## 节点的函数
```python
@onready var weapon = get_node("Label/Weapon")
weapon.get_path()		# 绝对路径
```
## input函数（当有按键按下等会调用）
```python
# 可以在 项目->项目设置->输入映射 中绑定action和按键
func _input(event):
    # 判断触发action的按键是否按下
    if event.is_action_pressed("action"):
        pass
    # 判断触发action的按键是否松开
    if event.is_action_released("action"):
        pass
```
## 变量
```python
var name = "john"
var name := "john"
var name: String = "john"

# 常量
const NAME = "john"

# 静态变量，和c++中的不同，GDScript中的静态变量子类和父类共享
static var name = "john"

# 动态类型
var name = "john"
name = 42
# 静态类型，自动推导为String
var name := "john"

# @export使得godot的inspector面板可以改变该变量
@export var name
@export_range(min: float, max: float, step: float = 1.0)
@export_category("name")

# self的含义和c++中的一样
func eat(n: Node):
    pass
eat(self)
```
## get和set
```python
var i := 42:
    set(value):
        i = value
    get:
        return i
```
## await
```python
await  get_tree().create_timer(0.075).timeout	# 延迟0.075秒
```
## 数据类型和类型转换
```python
bool, int, float, String, Array, Dictionary, Vector2, Vector3, Color

# 初始化Vector2
var vec = Vector2(10, 10)

# 类型转换
var name = str(42)
var i = int("42")

# as关键字
var i = 12.0 as int
```
## 数组
```python
var arr = [0, 1, "nihao"]
var arr: Array = [0, 1, "nihao"]
var arr: Array[int] = [0, 1, 2]
arr[0]
arr.apend()
arr.pop_back()		# 返回删除的元素
arr.pop_front()		# 返回删除的元素

arr.remove_at(0)
arr.insert(2, 5)	# 参数1/索引，参数2/元素
```
## 字典
```python
var dic = {"name": "john", "age": 18, 0: "zero"}
dic["name"]
dic["hobby"] = "swim"

# 使用for遍历字典，实际遍历的是键
for item in dic:
    print(dic[item])
dic.keys()
```
## 枚举
```python
# 定义
enum { ZERO, SECOND, THIRD }
enum Colors { RED, BLUE = 42, DARK }
var number = SECOND
var color = Colors.RED

# 使用@export使得godot的inspector面板可以改变该变量
@export var color : Colors
```
## 随机数
```python
var f = randf()					# 范围[0, 1]
var f = randf_range(2.3, 4,5)	# 范围[2.3, 4.5]
var i = randi() % 20			# 范围[0, 19]
var i = randi_range(-5, 5)		# 范围[-5, 5]


# clampi适合用于hp、speed等，只返回符合范围的数
var speed = 42
var i = clampi(speed, 1, 20)	# i为20
speed = -20
var i = clampi(speed, 1, 20)	# i为1
```
## 条件语句
```python
if 4 > 2:
    pass
elif 2 > 3:
    pass
else:
    pass

# c++中的switch
match 3:
    3:
        pass
    _:
        pass
    
# 三元运算符
var direction = -1 if state == "left" else "right"
    
# 可以使用is来判断变量类型
var i = 42
if i is int:
    pass
```
## 循环
```python
for number in range(3):
    pass

var arr = [0, 1, 2]
for number in arr:
    pass

while 4 > 2:
    pass

continue
break
```
## 信号
```python
# 定义信号
signal eat
signal swim(person)
# 释放信号
eat.emit()
swim.emit("John")
# 连接信号
eat.connect(函数)
```
## 函数
```python
# 定义
func run(name):
    print(name)
    return true
var res = run("john")

# 更详细的定义
func run(name: String) -> bool:
    print(name)
    return true
var res = run("john")


# _process函数每帧都会运行
func _process(delta):	# delta代表上一帧到现在的时间差，以秒为单位
    return

# _physics_process的间隔一致，每秒60帧
func _physics_process(delta):
    return

# _ready函数在初始化时运行一次
func _ready():
    pass

# _input函数在当有按键按下等情况下会调用
func _input(event):
    pass

# _unhandled_input
func _unhandled_input(event):
    pass
```
## 继承
```python
# (optional) icon to show in the editor dialogs:
@icon("res://path/to/optional/icon.svg")

# 每个文件就是一个类
class_name MyClass		# 类声明（可选）
extends BaseClass		# 继承

# 上面两句可以写成一行
class_name Myclass extends BaseClass
```
## inner class
```python
extends Node

# 创建一个实例
var chest := Equipment.new()
var legs := Equipment.new()

# inner class
class Equipment:
    var armor := 10
    var weigth := 5
```
# Godot
```python
queue_free()从游戏中移除当前节点

get_tree().reload_current_scene()重载当前场景，可用于玩家死亡后复活

Engine.time_scale = 0.5让时间变为原来的一半
```

```python
# 可以看作对资源的引用
const PLAYER = preload("res://player.scene")
var player = PLAYER.instantiate()
```

```python
# 惯例
func _process(delta):
    position.x += direction * SPEED * delta
```
## 人物scene
```python
# CharacterBody2D
# 	- AnimatedSprite2D
#	- CollisionShape2D
```
## 游戏scene
```python
# Node2D
# 	- CharacterBody2D
#		- Camera2D
#	- TileMapLayer
#	- AnimatableBody2D
```
## 移动平台scene
```python
# AnimatableBody2D
# 	- Sprite2D
#	- CollisionShape2D


# StaticBody2D

# AnimationPlayer
```
## Vector2
```python
var pos = Vector2.ZERO
```
## Input
```python
# 判断是否某个action被触发
Input.is_action_just_pressed("action")

# 如果action1被触发返回1，action2被触发返回-1，都没有触发返回0，适合用于方向
var direction = Input.get_axis("action1", "action2")

# 通常用于左键减右键，如何左右键同时按下，则direction为0
var direction = Input.get_action_strength("left") - Input.get_action_strength("left")
```
## Node
```python
# 不处理节点
process_mode = Node.PROCESS_MODE_DISABLED

# 属性


# 方法
Array[Node] get_children(include_internal: bool = false) const		# 返回所有子节点
```
## CharacterBody2D
```python
# 属性
Vector2 velocity				# 速度，默认Vector2(0, 0)


# 方法
bool move_and_slide()			# 依据velocity移动物体
```
## 文件
```python
# 打开文件
var file = FileAccess.open("D://a.json", FileAccess.WRITE)
# 写入一行
file.store_line("")
```
## Json
```python
# 序列化
var dic: Dictionary = {"name", "john"}
var s = JSON.stringify(dic)

# 创建JSON对象
var json = JSON.new()
# 反序列化
json.parse(s)
json.get_data()
```
