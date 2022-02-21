# 变量

- 三种类型：全局变量，局部变量，表中的域
- 多变量同时赋值：`a , b = 10 , 2*x`
- 交换值：`a , b = b , a`
- 函数返回值：`a, b = f()`，f()的第一个返回值给a，第二个返回值给b

## 全局变量

- 默认情况下，变量总是全局的
- 全局变量不需要声明
- 想删除一个全局变量，只需要将它赋值为nil

~~~lua
>print(b)
nil
>b = 10
>print(b)
10
>b = nil
~~~

## 局部变量

- 能避免命名冲突
- 访问局部变量速度比全局变量快

~~~lua
--默认lua的变量都是全局变量
a = 5
--局部变量现需要显式声明 local
local b =3

function joke()
    c = 1 -- global 
    local d = 2 -- local
    d = 2 -- 对local变量重新赋值
end
~~~



# 数据类型

| 数据类型 | 描述                                    |
| -------- | --------------------------------------- |
| nil      | 表示一个无效值，条件表达式中相当于false |
| boolean  | true/false                              |
| number   | 双精度类型的实浮点数                    |
| string   | 字符串                                  |
| function | 由c或者lua编写的函数                    |
| userdata | 表示任意存储在变量中的c数据结构         |
| thread   | 表示执行的独立线路，用于执行协同程序    |
| table    | 关联数组，索引可以是数字，字符串或表。  |

- nil：作比较时加双引号

~~~lua
type(x) -- nil
type(x) == nil --false
type(x) == "nil" -- true
~~~

- boolean：把false和nil看作假，其他都是真，包括0

- string：由双引号or单引号表示，也可以用两个方括号表示一块字符串

~~~lua
html = [[
a
aa
aaa
]]
~~~

- 对数字字符串进行算数操作时，lua会尝试将字符串转成数字

~~~lua
print("3"+4) -- 7
print("-2e2"*"6") -- -1200
~~~

- 字符串连接用`..`

~~~lua
print("a".."b") -- ab
~~~

- 用#计算字符串长度

~~~lua
len = "addaf"
print(#len) -- 5

print(#"asd") -- 3
~~~

- table的初始化

~~~lua
local tbl1 = {} -- 空表
local tbl2 = {"apple","pear"} -- 直接初始化
~~~

- table是一个关联数组，索引可以是数字或字符串

~~~lua
a = {}
a["key"] = "value"
key = 10
a[key] = 22
for k,v in pairs(a) do
    print(k..":"..v)
end
-- key:value
-- 10:22
~~~

- lua的默认初始索引是1
- 当table的索引是字符串

~~~lua
t["i"] = "hello"
print(t["i"])
--等价
print(t.i)
~~~

- lua的函数可存在变量里

~~~lua
function factorial1(n)
    if n == 0 then
        return 1
    else
        return n * factorial1(n-1)
    end
end

print(factorial1(5)) -- 120
factorial2 = factorial1
print(factorial2(5)) -- 120
~~~

- function可以以匿名函数的方式通过参数传递

~~~lua
function testFun(tab,fun)
    for k,v in pairs(tab) do
        print(fun(k,v))
    end
end

tab={key1="val1",key2="val2"}
testFun(tab,function(key,val)
    			return key.."="..val
  			end)
~~~

- thread：lua里最主要的线程是协同程序。协程和线程差不多，拥有自己独立的栈，局部变量，和指针，可以和其他协程共享全局变量和其他大部分东西
- 线程可以运行多个，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起才会暂停

# 循环

## 循环类型

- while：真时执行
- for：定次数执行
- repeat...until：结束条件为止
- 循环嵌套：上述循环互相嵌套

## 循环控制语句

- break：跳出当前循环
- goto：跳转到标签处

# 流程控制

- if

~~~lua
if(0)
then
    print("0 is true")
end
~~~

- if else
- if 嵌套语句

# 函数

~~~lua
-- 格式
optional_function_scope function function_name(arguments...) -- optional_function_scope决定函数全局or局部 局部加local
    function_body
    return result_params_comma_separated
end

function max(num1 , num2)
    if(num1 > num2)
        res = num1
    else 
        res = num2
    end
    
    return res
end

print(max(10,4))
~~~

- 函数可以作为参数传递

~~~lua
myprint = function(param)
    print("这个是"..param)
end

function add(num1,num2,functionPrint)
    res = num1+num2
    functionPrint(res)
end

myprint(10) -- 10
add(1,2,myprint) -- 3
~~~

- 多返回值

~~~lua
function maxmun(a)
    local mi = 1 -- 最大值的索引
    local m = a[mi] -- 最大值
    for i,val in ipairs(a) do
        if val > m then
            mi = m
            m = val
        end
    end
    return m,mi
end

print(maxmun({8，12，4，3，2})) -- 12 2
~~~

- 可变参数

~~~Lua
-- ...表示函数有可变参数
function add(...)
    local s = 0
    for i,v in ipairs{...} do
        s = s + v
    end
    return s
end

print(add(3,4,5,6,7)) -- 25
~~~

- 可变参数赋值给变量

~~~lua
function average(...)
    res = 0
    local arg = {...}
    for i,v in ipairs(arg) do
        res = res + v
    end
    print("传入了"..#arg.."个参数") -- # == .length
    return res/#arg
end

print("平均值是"..average(12,3,32,45))
~~~

