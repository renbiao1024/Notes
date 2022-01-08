## 关键字

![image-20220108102937622](c++小结.assets/image-20220108102937622.png)

## 替代标记

![image-20220108103032433](c++小结.assets/image-20220108103032433.png)

## ASCII字符集

<img src="c++小结.assets/image-20220108103207919.png" alt="image-20220108103207919"  />

<img src="c++小结.assets/image-20220108103225576.png" alt="image-20220108103225576" style="zoom:150%;" />

<img src="c++小结.assets/image-20220108103316990.png" alt="image-20220108103316990" style="zoom:150%;" />

## 运算符的优先级

![image-20220108103419712](c++小结.assets/image-20220108103419712.png)

<img src="c++小结.assets/image-20220108103434015.png" alt="image-20220108103434015" style="zoom:150%;" />

![image-20220108103456920](c++小结.assets/image-20220108103456920.png)

## 按位运算符

<<  >> & | ^ ~

- << 和 >>

*value* << *shift* 

其中，value 是要被操作的整数值，shift 是要移动的位数。

![image-20220108104000428](c++小结.assets/image-20220108104000428.png)

![image-20220108104025296](c++小结.assets/image-20220108104025296.png)

- ~

非运算符（!）和位非（或求反）运算符（～）。!运算符将 true（或非零值）转换为 false，将 false 值转换为

true。～运算符将每一位转换为它的反面（1 转换为 0，0 转换为 1）。

![image-20220108104040047](c++小结.assets/image-20220108104040047.png)

- |或

![image-20220108104052583](c++小结.assets/image-20220108104052583.png)

- ^ 异或

![image-20220108104142270](c++小结.assets/image-20220108104142270.png)

- &与

![image-20220108104259420](c++小结.assets/image-20220108104259420.png)

## string方法

![image-20220108110121747](c++小结.assets/image-20220108110121747.png)

![image-20220108110232787](c++小结.assets/image-20220108110232787.png)

## 容器方法

![image-20220108110518956](c++小结.assets/image-20220108110518956.png)

![image-20220108110532128](c++小结.assets/image-20220108110532128.png)

![image-20220108110545689](c++小结.assets/image-20220108110545689.png)

![image-20220108110559889](c++小结.assets/image-20220108110559889.png)

![image-20220108110610052](c++小结.assets/image-20220108110610052.png)

![image-20220108110618515](c++小结.assets/image-20220108110618515.png)

![image-20220108110626802](c++小结.assets/image-20220108110626802.png)

![image-20220108110637539](c++小结.assets/image-20220108110637539.png)

![image-20220108110647399](c++小结.assets/image-20220108110647399.png)

![image-20220108110703197](c++小结.assets/image-20220108110703197.png)

![image-20220108110714425](c++小结.assets/image-20220108110714425.png)

![image-20220108110724189](c++小结.assets/image-20220108110724189.png)

![image-20220108110733914](c++小结.assets/image-20220108110733914.png)

![image-20220108110749188](c++小结.assets/image-20220108110749188.png)

![image-20220108110800575](c++小结.assets/image-20220108110800575.png)

![image-20220108110842926](c++小结.assets/image-20220108110842926.png)

![image-20220108110903225](c++小结.assets/image-20220108110903225.png)

## 标准c++

~~~cpp
//使用 const 而不是#define 来定义常量
#define Max_Size 100
const int Max_Size = 100;//better

//使用 inline 而不是# define 来定义小型函数
#define Cube(X) X*X*X
inline int ClassName::Cube(x)const
{
	return x*x*x;
}

//尽可能在函数原型和函数头中使用 const。接受 const指针或引用的函数能够同时处理 const 数据和非 const 数据，而不使用 const 指针或引用的函数只能处理非const 数据

//malloc()和 free()，请改用 new 和 delete

//使用 setjmp()和 longjmp()处理错误，则请改用 try、throw 和 catch。

//使用智能指针来保证new d
~~~

