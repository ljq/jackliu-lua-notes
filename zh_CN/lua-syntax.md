# LUA Notes

### 注释

* 单行注释  
```--```
* 多行注释  
```
--[[ 
    注释
--]]
```
或  
```
--[[ 
    注释
  ]]
```
* **多行注释遇到用[[和]]表示的字符串就会提前结束，可以用----[=[ 注释内容 ]=]来解决**  
```
--[=[ 
    注释
  ]=]
```





### 程序块chunk

##### lua语法-程序块（chunk）
* lua解释器以程序块的方式处理lua代码
* 每一段可执行的lua代码都可以成为程序块
* lua程序块指一条或多条合法的可执行语句
* 一个程序块由一条或多条lua语句构成
* 简单的程序块：一条语句
* 复杂的程序块：多条不同语句及函数定义构成
**语句结尾的分号缺省，如一行多哥语句，建议分号隔开**





### 数据类型

##### table （创建不同的数据类型：数组、字典等）
* table 数据结构本身支持多态，定义灵活,是数组和集合的混合物
* table 使用关联数组，可以使用人艺类型值作索引，但值不能为nil
* table 是不固定大小的，你可以哥怒自己需要进行扩展
* lua的模块（mobule）、包（package）、和对象（Object）本身也是基于table的

```
t = {
    "a", 
    "b", 
    "c", 
    [4] = "d", 
    [5] = "f"，
    [6] = function(){}
}
``` 

##### Table 操作

| 操作 | 方法 | 用途 |
|:----:|:----|:-----------|
| 连接 | table.concat (table [, sep [, start [, end]]]) | concat是concatenate(连锁, 连接)的缩写. table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素, 元素间以指定的分隔符(sep)隔开。|
| 新增 |  table.insert (table, [pos,] value) | 在table的数组部分指定位置(pos)插入值为value的一个元素. pos参数可选, 默认为数组部分末尾.| 
| ~~maxn~~	| ~~table.maxn (table)~~ | 指定table中所有正数key值中最大的key值. 如果不存在key值为正数的元素, 则返回0。(~~**Lua5.2之后该方法已经不存在了**~~) | 
| 移除 | 	table.remove (table [, pos]) | 返回table数组部分位于pos位置的元素. 其后的元素会被前移. pos参数可选, 默认为table长度, 即从最后一个元素删起。| 
| 排序 | table.sort (table [, comp]) | 对给定的table进行升序排序。| 





### 变量

* **默认变量为全局变量**，包含方法内变量，如果设置局部变量，则添加修饰符local(此处极易导致异常和Bug，谨慎使用，**尽可能使用局部变量**)，使用局部变量的好处：
    - 避免命名冲突和和变量误覆盖
    - 访问局部变量的速度更快
    - 避免逻辑错误缺陷

* 全局变量删除：只需要将变量赋值为nil。





### 运算符

##### 运算符号
* 关系运算符: ```<``` 、```>```、```<=``` 、```>=``` 、 ```==``` 、 ```~=```(**不等于**)
* 逻辑运算符: ```and or not```
* 连接运算符: .. (两个点)

##### 运算注意事项
* "0" 和 0 (false,不相等)
* nil只等于nil本身
* 逻辑运算符false和nil是假(false),其他为true（包括0也是true）
* and和or的运算结果不是true和false，而和两个操作数相关：
    * ```a and b``` -- 如果a为false，则返回a，否则返回b
    * ```a or b``` -- 如果a为true，则返回a，否则返回b
* lua比较数字按传统的数字大小进行，比较字符串按字母的顺序进行（但字母顺序依赖于本地环境(字符编码或其他顺序等不同)）





### 函数

* ```()``` 括号仅用于函数中使用，其他地方可缺省忽略





### 循环

##### while
```
while(condition)
do
   statements
end
```

##### for
```
for var=exp1,exp2,exp3 do  
    --
end  
```

##### repeat
```
repeat
   statements
until( condition )
```

##### break 和 goto

goto语法格式：```goto Label```; Label 的格式为```:: Label ::```
```
local a = 1
::label:: print("--- goto label ---")

a = a+1
if a < 3 then
    goto label   -- a 小于 3 的时候跳转到标签 label
end

```

##### continue官方未内置，可通过其他方式实现

不提供continue的原因：  
Lua-FAQ：  
This is a common complaint. The Lua authors felt that continue was only one of a number of possible new control flow mechanisms (the fact that it cannot work with the scope rules of repeat/until was a secondary factor.)  
Lua作者认为continue非必要特性且只是许多新控制流机制中的一种，未加入此特性，continue关键字在repeat/until结构中会引发出现一些问题。  

##### lua中模拟“continue”的几种方法：

* 使用repeat循环包住需要要continue跳过的代码，使用break跳出循环, 需要注意的是，lua中的repeat语句，在循环条件为真的时候退出
```
for i = 1, 10 do
    repeat
        if i%2 == 0 then
            break
        end
        print(i)
        break
    until true
end
```

* 使用while循环包住需要continue跳过的代码， 使用break跳出循环
```
for i = 1, 10 do
    while true do
        if i%2 == 0 then
            break
        end
        print(i)
        break
    end
end
```

* 在lua5.2版本之后，可以使用goto语句来模拟
```
for i = 1, 10 do
    if i%2 == 0 then
        goto continue
    end
    print(i)
    ::continue::
end
```

### 模块与包

* local 对象，外部模块无法访问

* 环境变量自定义设置  

**文件路径以 ";" 号分隔，最后的 2 个 ";;" 表示新加的路径后面加上原来的默认路径。**

注意事项：  
Mac OS 系统下，```export LUA_PATH="~/lua/?.lua;;"```在.zshrc中如果在用户home路径下自定义模块路径，
会被当成～字符，不会解析用户的home路径```no file '~/lua/modules/module.lua'```，故建议应该写为绝对路径。
```
#LUA_PATH
#export LUA_PATH="~/lua/?.lua;;"
export LUA_PATH="/Users/iotd/lua/?.lua;;"

```

```
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
 -- 外部无法访问到
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module
```

