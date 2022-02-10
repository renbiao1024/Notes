# 条款01：视c++为一个语言联邦

**c++高效编程守则视状况而变化，取决于你使用c++哪一部分**

-  主要次语言

  - c

  - c with classes：面向对象编程

  - template c++：泛型编程

  - STL：template程序库

- 对于内置类型来说，pass-by-value通常比pass-by-reference高效
- 对于class里用户自定义的构造函数和析构函数，pass-by -reference更好

# 条款02：尽量以const,enum,inline代替#define

**对于单纯常量，最好以const或enum替换#define**

**对于形似函数的宏，最好改用inline函数替换#define**

- 换言之：尽量用编译器代替预处理器

- 如`#define PI 3.14`在预处理阶段就已经将所有的PI替换成3.14，PI不会进入记号表。因此报错时不会提示PI，而是3.14，容易造成困惑，追踪他浪费时间。
  - 解决之道：`const double PI = 3.14;`编译器一定能接收PI到记号表内，避免了#define盲目将PI替换产生多份3.14

- class专属常量，为了确保只有一份实体，需要声明为static 

~~~cpp
//.h
class A{
 private:
    static const int s = 2;
};
//.cpp
const int A::s ;
~~~

- enum hack
  - 比较像define，无法用指针或者引用找到该常量的地址，不会有非必要的内存分配

~~~cpp
//当编译器需要在编译期间知道该数值，且你的编译器不允许static类内常量完成初值设定，可用enum hack 补偿做法
class A{
 private:
    enum{Num = 5};//让num成为一个5的记号
    int score[Num];
};
~~~

- inline：拥有宏定义的效率和一般函数的所有课预料行为和类型安全性
  - `#define CALL_WITH_MAX(a,b) f((a)>(b)?(a):(b))`define定义宏需要给每个参数加括号，但仍然有出问题的可能

~~~cpp
template <typename T>
inline void CallWithMax(const T&a,const T&b)
{
	f(a>b?a:b);
}
~~~

- 我们对与编译器的需求降低了，但并非完全消除。#include仍是必需品，#ifdef/#ifndef控制编译

# 条款03：尽可能使用const

**将某些东西声明为const可以帮助编译器甄别出错误用法。**

**当const 和 non-const成员函数有者实质等价的实现，可以用non-const版本调用const版本避免代码重复**

- 只要某个值需要语义约束（保持不变），就该告诉编译器。

  - const出现在\*左边表示被指物是常量，const出现在*右边表示指针是常量

  - ~~~cpp
    const int * x;
    int const * x;
    //等价
    ~~~

- 迭代器 

~~~cpp
std::vector<int> vec;
std::vector<int>::iterator it = vec.begin();// it == int*
const std::vector<int>::iterator itc = vec.begin();// == int* const，指向不能改变
std::vector<int>::const_iterator cit = vec.begin();// == const int*,所指的东西不能改变
~~~

- 可以避免类似  将`==`打成` =` 的错误

- const成员函数

  - 两个成员函数如果只是常量性不同，可以被重载

  - 常成员函数：const返回值，且不能修改函数成员

~~~cpp
class CTextBlock{
public:
    std::size_t length()const;
private:
    char* pText;
    //std::size_t textLength;
    //bool lengthIsValid;
    mutable std::size_t textLength;
    mutable bool lengthIsValid;
};

std::size_t CTextBlock::length()const//常成员函数
{
    if(!lengthIsValid)
    {
        textLength = std::strlen(pText);//常成员函数要想修改函数成员，需要在成员嵌加mutable，表示可变的
        lengthIsValid = true;
    }
    return textLength;
}
~~~

- 用const成员函数实现non-const版本,反过来不允许。

~~~cpp
class TextBox{
public:
    const char& operator[](std::size_t position)const
    {
		...
        return text[position];
    }
    
    char& operator[](std::size_t position)
    {
        return const_cast<char&>(static_cast<const TextBox&>(*this)[position]);
    }
};
~~~

# 条款04：确定对象在使用前被初始化

**为内置对象手工初始化，因为c++不能保证初始化他们**

**构造函数最好使用成员初值列，而非在构造函数里赋值。初值列的成员变量的排列次序应该和声明次序相同**
**为免除“跨编译单元之初始化次序”的问题，请以local static对象替换non local static 对象**

- 以**函数调用（返回一个reference指向local static对象）**替换**直接访问non-local static对象**，从而保证获得的reference是一个被初始化过的对象

~~~cpp
class A{
    size_t nums();
};
A& ref_A(){
    static A ref_a;
    return ref_a;
}

class B{};
B::B()
{
	size_t res = ref_A().nums();//使用的是指向static对象的引用，而非对象本身
}
~~~

# 条款05：了解c++默认编写并调用的函数

- 空类

~~~cpp
class Empty{};

//编译后，变成
cclass Empty{
public:
    Empty();
    Empty(const Empty&rhs);
    ~Empty();
    Empty& operator=(const Empty& rhs);
};
~~~

- 如果类内有reference，const成员，生成赋值操作符会修改他们，不合法。编译器就不会自动生出这个操作符

# 若不想使用编译器自动生成的函数，就该明确拒绝

**为了驳回编译器自动提供的机能，可将相应的成员函数声明为private并不予实现，或者继承自这样的base class**

- 当一个类不希望被复制，就不该有编译器自产的拷贝构造函数
  - 解决方法：将拷贝构造函数，拷贝操作符声明为private，并且不实现他们。

~~~cpp
//方法一
class NonCopy{
private:
    //只做声明
    NonCopy(const NonCopy&);
    NonCopy& operator=(const NonCopy&);
};

//方法二
class Uncopyable
{
protected:
    Uncopyable(){}
    ~Uncopyable(){}
    
private:
    Uncopyable(const Uncopyable&);
    Uncopyable& operator=(const Uncopyable&);
};

class Son :private Uncopyable{
    
};
~~~

# 为多态基类声明virtual析构函数

**带多态性质的基类应该声明一个虚析构函数  **

**如果clas带有任何virtual函数，就该有一个virtual析构函数**

**如果class设计的目的不是为了作为base class 使用，或者不是为了具有多态性，就不该声明virtual析构函数**

- 基类析构函数不加virtual时，析构时派生类可能析构失败

~~~cpp
class TimerKeeper
{
public:
    TimerKeeper();
    virtual ~TimerKeeper();
};

TimerKeeper*ptk = getTimerKeeper();

delete ptk;
~~~

- 当类里没有virtual，通常意味着他不被意图用作一个基类。在非基类给析构函数使用virtual是个馊主意，增加了虚表指针和虚表的开销，和其它语言的移植性也会降低。通常类内至少有一个virtual函数时才将析构函数声明为virtual