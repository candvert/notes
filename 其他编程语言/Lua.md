```
不支持类
只有nil和false代表假，即使数字0也为真
不支持n--和n-=1
```
## 注释
```lua
-- 单行注释

--[[
     多行注释
--]]
```
## 变量
```lua
-- 默认所有变量为全局变量，可以在其他文件中使用，局部变量需local关键字
-- 所有未声明的变量为nil

local li = 10 --局部变量

num = 42  -- 所有的数字都是双精度浮点型。
s = 'walternate'  -- 和Python一样，字符串不可变。
t = "也可以用双引号"
u = [[ 多行的字符串
       以两个方括号
       开始和结尾。]]
t = nil  -- 撤销t的定义; Lua 支持垃圾回收。
a = true
b = false

a,b = 1,2 --可以同时为多个变量赋值
a = 0x11
a = 2e10
print(10^5) --即支持幂运算符

a = 'md'
b = 'ld'
c = a..b --c的值为a与b相连的结果，即mdld
-- 下面这些都可以连接字符串
c = a.. b
c = a ..b
c = a .. b
c = a   ..       b
print(#a) --输出字符串a的长度

c = tostring(10)
n = tonumber('10')
```
## 表table
```lua
a = {1,"ac",{},function() end}

-- lua语言中下标开始值是1，而不是其他语言中的0
print(a[1]) --输出1

-- 直接通过下标添加元素
a[5] = 456
-- 通过函数添加元素
table.insert(a,'ok')
table.insert(a,2,'nook') --插入到下标2的位置

-- 删除元素
s = table.remove(a,2)

print(#a) --输出数组长度

-- 用字符串作为下标，即字典
mp = {
	first = 2
	second = "hello"
	c = function() end
}

print(a['first']) --输出2
print(a.first) --输出2





-- lua中特殊的表_G，所有的全局变量都位于这个表中
a = 1
print(_G["a"]) --输出1
```
## 判断和循环
```lua
-- lua中不等于用~=表示

print(a and b)
print(a or b)
print(not b)

b = 0
print(b > 10 and 'yes' or 'no') --输出no


if 1>10 then
	print('1>10')
elseif 1<10 then
	print('1<10')
else
	print('no')
end

-- 第三个数为步长，即2为步长
-- 无法在循环体中修改i的值
for i=1,10,2 do
	print(i)
end

for i=10,1,-1 do
	print(i)
	if i == 5 then break end
end

while n>1 do
	if n == 5 then break end
	print(n)
	n = n - 1
end
```
## 函数
```lua
-- 两种定义方式
function f(a,b)
	print(a,b)
end

f = function(a,b)
	print(a,b)
end

-- 使用return返回值
function f(a,b)
	return a
end

-- 也可以返回多个值
function f(a,b)
	return a,b
end
```