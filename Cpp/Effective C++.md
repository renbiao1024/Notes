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

# 条款06：若不想使用编译器自动生成的函数，就该明确拒绝

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

# 条款07：为多态基类声明virtual析构函数

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

# 条款08：别让异常逃离析构函数

**析构函数绝对不要吐出异常。如果一个被析构函数调用的函数可能可能抛出异常，析构函数应该捕捉任何异常，然后吞下他们或者结束程序**

**如果客户端需要对某个操作函数运行期间抛出的异常作出反应，那么class应该提供一个普通函数（而非析构函数）执行该操作**

- 创建一个管理资源的类

~~~cpp
class DBConnection{
public:
    static DBConnection create();
    void close();
};

class DBConn{
public:
    ~DBConn{
        if(!closed)
        {
            try{db.close();}
            catch(...)
            {
                //方法1：什么都不写，表示遇到异常就吞掉

                //方法2：
                std::abort();//如果close抛出异常就结束
            }
        }

    }
    
    void close()//将close的责任从DBConn的析构函数转移到普通函数，析构函数是一个双重保险
    {
		db.close();
        closed = true;
    }
private:
    DBConnection db;
    bool closed;
};

DBConn dbc (DBConnection::create());//建立一个DBConnection对象交给DBConn管理
~~~

# 条款09：绝不在构造和析构的过程中调用virtual函数

**在构造和析构期间不要调用virtual函数，因为这类调用从不下降至派生类**

- 在构造期间，虚函数不会被视为虚函数
- 在析构期间，B的成员变量就成为未定义的状态，所以调用A的virtual函数
- 信息不能由基类向下传递，但可以由派生类向上传递

~~~cpp
//错误示例
class A
{
public:
    A(){
        vA();
    }
    
    virtual void vA(){}
};

class B:public A{
public:
    vA(){}
};

B b;//构造B的时候先构造基类A，但A的构造调用了虚函数，此时B还没构造，所以调用的是A的vA

//正确实例
class A
{
public:
    A(const string&s){
        vA();
    }
    //非虚函数
    void vA(){}
};

class B:public A{
public:
    B():A(s){}//通过B的构造函数给A传递参数
private:
    string s;
};
~~~

# 条款10：令operator=返回一个reference to *this

- 为了实现连续赋值

~~~cpp
class Widget
{
pubic:
    Widget& operator=(const Widget&rhs)
    {
        ...
        return *this;
    }
};
~~~

- 也适用于+=，-=，*=等赋值相关的符号

# 条款11：在operator=中处理“自我赋值”

**确保当对象自我赋值额时候operator=有良好的行为，其技术包括“来源对象”“目标对象”的地址，语句顺序以及”copy and swap“**

**确保函数操作多个对象，而多个对象是同一个对象时的行为仍然正确**

~~~CPP
class A{};
class B{
private:
    A*a;
};
//方法1
B::B& operator=(const B&rhs)
    {
        //自我测试
        if(this == &rhs) return *this;
        
        delete a;//没有自我检测时，如果a和rhs指向同一个对象，那么delete a 也会把rhs的a删除，导致rhs指向nullptr
        a = new A(*rhs.a);
        return *this;
    }
//方法2（better）,如果new的时候抛出异常，也不会删除原来的a
B::B& operator=(const B&rhs)
    {
        A* tmp = a;
        a = new A(*rhs.a);
    	delete tmp;
        return *this;
    }

//copy and swap技术
//方法3，直观
void swap(B&rhs);//交换*this和rhs
B::B& operator(const B&rhs)
{
	B tmp(rhs);
    swap(tmp);
    return *this;
}

//方法4：和方法三一样，但是效率更高
B::B& operator(const B rhs)//传值相当于	B tmp(rhs); 构建了一个副本
{
    swap(rhs);
    return *this;
}
~~~

# 条款12：复制对象时勿忘其每一个成分

**Copying函数应该确保复制“对象内的所有成员变量”及“所有base class成分”**

**不要尝试用一个copying函数调用另一个copying函数，应该将共同部分放到第三个函数里，由两个函数共同调用**

- 复制每一个成分包括
  - 赋值所有的local成员变量
  - 调用所有的父类复制函数

~~~cpp
class A
{
public:
	A(const A& rhs):name(rhs.name),age(rhs.age){}
    
    A& operator=(const A& rhs)
    {
        name = rhs.name;
        age = rhs.age;
        return *this;
    }
private:
    string name;
    int age;
};

class B :public A
{
private:
    int priority;
    
public:
    B(const B& rhs):A(rhs),priority(rhs.priority){}//派生类的构造函数需要调用父类的构造函数 并且要为自身的变量初始化
    B& operator=(const B&rhs){
        A::operator=(rhs);
        priority = rhs.priority;
        return *this;
    }
};
~~~

- 复制构造函数和复制操作符不能互相调用，如果有重复的代码可以另外书写一个init函数，让两个函数调用

# 条款13：以对象管理资源

**为防止资源泄露，请用RAII对象（资源获取即初始化），他们在构造函数中获得资源并在析构函数中释放资源**

**两个常用的RAII class是auto_ptr和 shared_ptr,前者会使被复制物指向null，后者的copy行为较佳**

- 手动释放资源可能引发内存泄漏，可以自定义资源管理类

- 使用智能指针来避免资源泄露的可能性

~~~cpp
class A{
public:
    A* createA();
};


std::auto_ptr<A>p1(createA());//获取资源后立刻放进管理对象，管理对象运用析构函数确保资源释放，当auto_ptr被销毁时，会自动删除它的所指之物，但要注意不能用多个autoptr指向同一个对象，防止多次释放。

//Autoptr的特殊性质：当复制操作后，新的对象会代替原来的对象，成为获得资源的唯一拥有权
std:;auto_ptr<A>p2(p1);//现在p2指向对象，p1指向null
p1 = p2;//现在p1指向对象，p2指向null
~~~

- RCSP引用计数型智能指针
  - 追踪有多少个对象指向某笔资源，无人指向时自动删除该资源
  - 无法打破环状引用

~~~cpp
std::tr1::shared_ptr<A>p3(createA());//p3指向createA返回值
std::tr1::shared_ptr<A>p4(p3);//p4 指向 p3 的对象
p3 = p4;//同上，无变化
//当p3 p4 都销毁 所指对象才销毁
~~~

- auto_ptr,shared_ptr不能处理数组，可以用boost::scoped_array，boost::shared_array类

# 条款14：在资源管理类中小心copying行为

**复制RAII对象必须一并复制他所管理的资源，所以资源的copying行为决定了RAII对象的copying行为**

**常见的RAIIcopying行为有“抑制copying”“引用计数法”“所有权转移”“深度拷贝”**

- auto_ptr和 shared_ptr适合处理堆上的资源，当遇到其他资源，需要自己建立资源管理类

~~~cpp
void lock(Mutex*pm);
void unlock(Mutex*pm);

class Lock{
public:
    explicit Lock(Mutex*pm):mutexPtr(pm){
        lock(mutexPtr);
    }
    ~Lock(){
        unlock(mutexPtr);
    }
private:
    Mutex*mutexPtr;
};

Mutex n;
{
    Lock m1(&m);
    ...
        //在区块末自动调用析构函数解锁
}
~~~

- 面对RAII对象被复制的选择

  - 禁止复制

  ~~~cpp
  class Lock:private Uncopyable{//条款6
    ...  
  };
  ~~~

  - 对底层资源祭出“引用计数法”

  ~~~cpp
  //shared_ptr的删除器（第二参数）当引用计数为0时调用
  class Lock{
  public:
      explicit Lock(Mutex*pm):mutexPtr(pm,unlock)
      {
          lock(mutexPtr.get());
      }
      //使用缺省析构函数
  private:
      std::tr1::shared_ptr<Mutex>mutexPtr;
  };
  ~~~

  - 复制底部资源：深拷贝
  - 转移底部资源的拥有权：拥有权从被复制物转移到目标物

# 条款15：在资源管理类中提供对原始资源的访问

**APIs往往要求访问原始资源，所以每个RAIIclass都应该提供一个“取得其所管理资源”的方法**

**对原始资源的访问可能经由显式转换或者隐式转换，一般而言，显式转换安全，隐式转换方便**

- 如果一个函数需要的是原属数据，在使用RAII管理类后不能直接传入
- 显式转换

~~~cpp
int days(const A* pi);

std::tr1::shared_ptr<A>p(createA());//createA() return A*
//int d = days(p)//error,不能传入一个shared_ptr类型
int d = days(p.get());//get提供一个显式钻换，返回指针内的原始指针的复件
~~~

- 隐式转换

~~~cpp
class Font{
public:
    //    FontHandle get()const {return f;} //显式转换函数
    operator FontHandle()const{return f;}//隐式转换函数

private:
    FontHandle f;
};

Font f1(getFont());
//Font f2 = f1;//error,f1被隐式转换为FontHandle
FontHandle f2 = f1;//实现了自动隐式转换，但影响了使用原来的类
~~~

# 条款16：成对使用new和delete时要采用相同的形式

**new中使用[]，必须在相应的delete中也使用[]。如果new的时候不使用[]，delete也不用**

~~~cpp
typedef string AddressLines[4];
std::string* pal = new AddressLines;//equal to  string*pal = new string[4]
delete []pal;
~~~

# 条款17：以独立语句将newed对象置入智能指针

**以独立语句将newed对象存储在智能指针内。如果不这样做，一旦异常被抛出，有可能导致难以察觉的资源泄露**

~~~cpp
processWidget(std::tr1::shared_ptr<widget>(new widget),priority());//这种写法，不能保证三个操作的执行顺序，processWidget操作异常时可能无法将new widget放到shared_ptr管理，造成资源泄露

//独立语句没有这个问题
std::tr1::shared_ptr<widget>pw(new widget);

processWidget(pw,priority());
~~~

# 条款18：让接口容易被正确使用，不容易被误用

**"促进正确使用"的办法包括接口的一致性，以及与内置类型的行为兼容**

**”阻止误用“的办法包括建立新类型，限制类型上的操作，束缚对象值，消除客户端的资源管理责任**

**shared_ptr支持定制型删除器，可用于防范DLL问题，可悲用来自动解除互斥锁等等**

~~~cpp
class Date{
public:
    Date(int month,int day,int year);
    //客户端可能顺序错误的传参，也可能传入无效日期
};

//导入新的类型来防范
struct Day{
    explicit Day(int d):val(d){}
    int val;
};
//Month Year同理

//限制范围
class Month{
public:
    static Month Jan(){return Month(1);}
    static Month Feb(){return Month(2);}
    //其他月...
private:
    explicit Month(int m);
};
class Date{
public:
    Date(const Month&m,const Days&d,const Year&y);
};

Date d(Month::Feb(),Day(21),Year(1999));
~~~

- 任何借口如果要求客户端必须记得做某事，就是有“不正确使用”的倾向。
  - 如资源管理

~~~cpp
std::tr1::shared_ptr<book>createBook()
{
	std::tr1::shared_ptr<book>retVal(static_cast<book*>(0),deleter);//创建资源管理
    retVal = ...;//指向正确值
    return retVal;
}
~~~

# 条款19：设计class犹如设计type

设计class的注意点

- 新type的对象应该如何被创建和销毁：涉及构造函数，析构函数，内存分配函数和释放函数
- 对象的初始化和赋值应该有什么样的差别：决定构造函数和赋值操作符的行为
- 新type的对象如果按值传递：涉及拷贝构造函数
- 新type的合法值：决定类必须维护的约束条件
- 新type是否需要配合某个继承图系：类有继承关系会受到继承类的约束
- 新type需要什么样的转换：是否允许隐式转换，还是只允许explicit构造函数存在，就需要专门的执行转换的函数
- 什么操作符和函数对此新type合理
- 应该驳回什么样的标准函数：private者
- 谁该用新的type成员：用限定符管理，public，private，friend，protected
- 新type的未声明接口：为效率，异常安全性，资源运用提供约束条件
- 新type有多么一般化：class or template class
- 真的需要新的type吗：继承方便还是新的方便

# 条款20：用pass-by -reference-to-const 代替pass-by-value

**尽量用pass-by -reference-to-const 代替pass-by-value，前者通常比较高效，并且可以避免切割问题**

**以上规则不适用于内置类型和STL的迭代器和函数对象，对他们而言，pass-by-value更合适**

- 传值会比传引用多一次构造和析构操作，内部的参数也要完全拷贝一份，资源消耗很大

- 在继承关系上，传值会造成对象切割

~~~cpp
class A{
public:
    std::string name()const;
    virtual void display()const;
};

class B:public A{
public:
    virtual void display()const;
};

void printNameAndDisplay(A a)
{
    std::cout<<a.name();
    a.display();
}

B b;
printNameAndDisplay(b);//b会被构造成一个A对象，所有B特化的信息都会被切除
~~~

- 内置类型 pass by value 的效率王王比pass by reference to const高，STl的迭代器和函数对象就被设计为pass by value

# 条款21：必须返回对象时，别妄想返回其reference

**绝不要返回一个pointer或者reference指向一个local stack对象，或者返回reference指向一个heap-allocated对象，或者返回pointer或reference指向一个local static对象而有可能需要多个这样的对象**

~~~cpp
inline const A operator*(const A&lhs,const A&rhs)
{
    return A(lhs.n*rhs.n);
}//要返回新的对象，就要付出构造和析构的代价，但是只能这样。编译器可能会安全消除该行为，程序执行比预期要快
~~~

# 条款22：将成员变量声明为private

**切记将成员变量声明为private。这可以赋予客户访问数据的一致性，可惜话访问控制，允许约束条件获得保证，并提供class作者以充分的实现弹性**

**protected，public都不具有封装性，private具有封装性**

- 成员变量的控制

~~~cpp
class AccessLevels{
public:
    int getReadOnly()const{return readOnly;}
    void setReadWrite(int val){readWrite = val;}
    int getReadWrite()const{return readWrite;}
    void setWriteOnly(int val){writeOnly = val;}
private:
    int noAccess;
    int readOnly;
    int readWrite;
    int writeOnly;
};
~~~

- 封装：public意味着不封装，修改这个变量影响的范围很广，protected成员一旦修改，派生类都受到影响，private时提供封装的访问权限

# 条款23：宁以non-member,non-friend替换member函数

**以non-member,non-friend替换member函数。这样可以增加封装性，包裹弹性和技能扩充性**

- 成员函数或者友元函数都能访问private数据，不利于封装性

~~~cpp
namespace WBS{
   class WebBrowser{};
    void clearBrowser(WebBroser&wb);//无权访问WebBrowser的private
}
~~~

- 将所有便利函数放在多个头文件里，但在一个namespace下。这些便利函数就可以整合在一起

# 条款24：若所有参数皆需要类型转换，请为此采用non-member函数

