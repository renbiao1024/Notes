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
    Lock m1(&n);
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

**需要为某个函数的所有参数（包括被this指针所指的那个隐喻参数）进行类型转换，那么这个函数必须是一个non-member**

~~~cpp
class A{
public:
    explicit const A operator*(const A&rhs)const;//1，只允许显式转换
    
    const A operator*(const A&rhs)const;//2，只有rhs一个参数可以隐式转换
};

const A operator*(const A&lhs,const A&rhs)const;//3，两个参数都可以隐式转换

A a(1);
A b = a * 2;//A b = a.operator*(2);//2(隐式转换),3可通过编译
A c = 2 * a;//A c = 2.operator*(a) //3(隐式转换)可通过编译
//所以应该使用3方法
~~~

# 条款25：考虑写一个不抛出异常的swap函数

**当std::swap对你的类效率不高时，提供一个swap成员函数，并确定这个函数不抛出异常**

**如果提供一个member swap函数，也该提供一个non-member swap来调用之。对于classes，也请特化std::swap**

**调用swap时应针对std::swap使用using声明式，然后调用swap不带任何命名空间修饰**

**为“用户自定义类型”进行std::templates 全特化是好的，但千万别尝试在std内家入某些对std而言全新的东西**

~~~cpp
namespace WS{
    template <typename T>
    class Widget{
    public:
        void swap(Widget& other)
        {
            using std::swap;//曝光std swap给当前函数
            swap(pImpl,other.pImpl);//当前命名空间有swap就调用当前的，否则调用std的
        }
    private:
        WidgetImpl*pImpl;
    };

    template<typename T>//特化
    void swap<Widget>(Widget<T>& a,Widget<T>& b)
    {
        a.swap(b);
    }
}
~~~

- 提供一个public swap成员函数，让他高效置换class类型的两个对象值
- 在class所在的namepace提供一个non-member swap，并令它调用上述swap成员函数
- 如果正在编写一个类，为你的类特化std::swap，并让他调用你的swap成员函数

# 条款26：尽可能延后变量定义式出现的时间

**尽可能延迟变量定义式的出现，这样可以增加程序的清晰度并改善程序效率**

- 定义延迟到首次需要使用他，甚至到首次初始化他

~~~cpp
string passWord(const string&key)
{
    //string password;
	if(key.size() < MinLength)
        throw logic_error("Password is too short");
    string password;//不定义在前面的原因是，只有当if不执行才需要用到，提前创建可能用不到，浪费了构造和析构的时间
    return password;
}
~~~

# 条款27：尽量少做转型动作

**如果可以，尽量避免转型，特别是在注重效率的代码中避免dynamic_casts，如果有个涉及需要转型动作，试着发展无需转型的代替设计**

**如果转型是必要的，试着将他隐藏在某个函数背后。客户端可以随时调用该函数，而不需要将转型放在他们自己的代码**

**宁可使用新式转型，不要使用旧时转型。前者易于辨识且有分门别类地掌职**

- 旧式转型
  - (T)expression:将expression转成T
  - T(expression):将expression转成T
- 新式转型
  - const_cast<T>(expression)：常用于常量性移除，只有这个转型可以做到这一点
  - dynamic_cast<T>(expression)：常用于安全向下转型
  - reinterpret_cast<T>(expression)：意图执行低级转型（point->int），实际动作可能取决于编译器
  - static_cast<T>(expression)：用来强迫隐式转换

~~~cpp
class Window{
public:
    virtual void onResize(){}
};

class SpecialWindow:public Window{
public:
    virtual void onResize(){
        //static_cast<Window>(*this).onResize();
        //这种方法会调用父类的副本执行onResize()，在执行自己的onResize(),最终导致父类实际没变，子类发生改变，半残状态
        
        //正确做法
        Window::onResize();//在本体调用父类的onResize
    }
};
~~~

- 使用容器并在其中存储直接指向派生类对象的指针，如此便消除了“通过基类接口处理对象”的需要

~~~cpp
class Window{};
class SpecialWindow:public window{
public:
    void blink();
};

typedef vector<tr1::shared_ptr<SpecialWindow>> VPSW;
VSPW winPtrs;

for(VPSW::iterator iter = winPtrs.begin();iter != winPtrs.end(); ++ iter)
{
    (*iter)->blink();
}
~~~

- 可以“通过基类接口处理对象”的派生类

~~~cpp
class Window{
public:
    virtual void blink(){}
};
class SpecialWindow:public window{
public:
    virtual void blink(){}
};

typedef vector<tr1::shared_ptr<Window>> VPW;
VPW winPtrs;

for(VPSW::iterator iter = winPtrs.begin();iter != winPtrs.end(); ++ iter)
{
    (*iter)->blink();
}
~~~

# 条款28：避免返回handles指向对象内部成分

**避免返回handles（指针，引用，迭代器）指向对象内部。防止破坏封装性，帮助const成员函数的行为像个const，并将发生“虚吊handles”的可能性降到最低**

- 指针，引用，迭代器统统都是handles,返回一个“代表对象内部数据”的handle，会有降低封装性的风险

~~~cpp
class Point{
public:
    Point(int x,int y);
    
    void setX(int val);
    void setY(int val);
};

struct PectData{
    Point ul;
    Point lr;
};

class Rectangle{
public:
    Point& upperLeft()const{return pData->ul;}//指向对象内部，通过这个引用可以修改ul
    const Point& lowerRight()const{return pData->lr;}//const引用不可修改
    
private:
    tr1::shared_ptr<RectData>pData;
};

Point coord1(0,0);
Point coord2(100,100);
const Rectangle rec(coord1,coord2);
rec.upperLeft().setX(50);//访问并修改了内部参数
rec.lowerRight().setX(50);//不能修改内部参数
~~~

- 当handles比其所指的对象更长寿，就可能发生虚吊

# 条款29：为“异常安全”而努力是值得的

**异常安全函数即使发生异常也不会泄露资源或允许任何数据结构败坏。这样的函数区分为三种可能的保证：基本型，强烈型，不抛异常型**

**强烈保证往往能够以copy and swap实现出来，但强烈保证并非对所有函数都可以实现或具备现实意义**

**函数提供的“异常安全保证”通常最高值等于其所调用的各个函数的异常安全保证的最弱者**

- 带有异常安全性的函数会
  - 不泄露任何资源
  - 不允许数据败坏

- 异常安全函数提供以下三个保证之一：

  - 基本承诺：如果异常被抛出，程序内任何事物仍保持在有效状态下，没有任何对象或数据结构因此败坏，所有对象都处于一种内部前后一致的状态。
  - 强烈保证：如果异常被抛出，程序状态不改变。调用这样的函数需要有这样的认知：如果函数成功，就完全成功；如果函数失败，程序就返回到调用函数前的状态。

  ~~~cpp
  class PrettyMenu{
      std::tr1::shared_ptr<image>bgImage;
  };
  
  void PrettyMenu::changeBackground(std::istream& imgSrc)
  {
      Lock m1(&mutex);//条例14的方法创建Lock管理类
      bgImage.reset(new Image(imgSrc));
      ++imgChanges;
  }
  ~~~

  

  - 不抛掷保证：承诺绝对不抛出异常，因为他们总是能够完成他们原先承诺的功能。

# 条款30：透彻了解inlining的里里外外

**将大多数inline限制在小型，被频繁调用的函数上。这可使日后调式过程和二进制升级更容易，也可使潜在的代码膨胀问题最小化，使程序的速度提升机会最大化**

**不要只因为function templates出现在头文件，就将他们声明为inline**

- 对此函数的每次调用，都以函数本体替换之

- 特点：
  - 执行相关语境最优化
  - 程序体积变大，导致额外的换页行为，降低高速缓冲装置的击中率
  - 当inline函数改变，所有使用它的地方都需要重新编译，而非内联函数只需要重新连接

- 结合上诉特点，inline函数适合函数本体的产出码比函数调用的产出码更小的情况
- inline是个申请，编译器可以忽略
  - 编译器拒绝复杂函数的inline，如带有循环，递归
  - 拒绝virtual函数，因为inline在编译阶段，virtual在运行阶段确定

~~~cpp
class Person{
public:
    //类内函数隐喻inline申请，类内的friend也有
    int age()const{return theAge;}
    
private:
    int theAge;
}
//显式申请inline
template<typename T>
inline const T& std::max(const T& a,const T& b){return a<b? b:a;}
~~~

# 条款31：将文件间的编译依存关系降到最低

**支持“编译依存最小化”的一般构想是：相依于声明式，不要相依于定义式，两个手段：handle class和interface class**

**程序的库头文件应该以“完全且仅有声明式”的形式存在。这种做法不论是否涉及templates都适用**

- 以声明的依存性，替代定义的依存性

~~~cpp
//当date类修改，person类也要重编译
class Date;
class Person{
public:
	string getDate()const;
    Person(Date&birthdate):theBirthdayDate(birthdate){}
private:
    Date theBirthdayDate;
};

//如何降低依赖性

//方法1：Handle class
//PersonImpl负责实现接口，Person提供接口，此时date修改不会导致person重编译
class PersonImpl;
class Date;
class Person{
public:
    Person(const Date&birthdate):pImpl(new PersonImpl(birthdate)){}
	string getDate()const
    {
        return pImpl->date();
    }
private:
    tr1::shared_ptr<PersonImpl>pImpl;
};

//方法2：Interface class
//实现接口类
class Person{
public:
    virtual ~Person();
    virtual string getDate() const = 0;
    
    static tr1::shared_ptr<Person>create(const Date& birthday)
    {
        return tr1::shared_ptr<Person>(new RealPerson(birthday));//返回有实现的子类
    }
};

//子类
class RealPerson{
public:
    RealPerson(const Date& birthday):theBirthdayDate(birthday){}
    
private:
    Date theBirthdayDate;
};
	//客户端使用
Date dateOfBirthday;
tr1::shared_ptr<Person>pp(Person::create(dateOfBirthday));
cout<<pp->getDate();
~~~

- 如果使用reference和pointer能完成任务，就不傲使用object
- 尽量以声明式代替定义式(前置声明)
- 为声明式和定义式提供不同头文件

# 条款32：确定你的public继承塑膜出is-a关系

**public继承意味is-a。适用于base class的事情也一定适用于derived class 身上，因为每个派生类也都是一个基类**

- 宁可采取“在编译期拒绝”，而不是“在运行期侦测他们”

~~~cpp
//在编译期拒绝
class Bird{
};

class FlyBird:public Bird{
public:
    void fly();
};
class Penguin:public Bird{};

//在运行期侦测他们
class Bird{
public:
    void fly();
};

class Penguin:public Bird{
public:
    void fly(){error("且不会飞");}   //产生一个运行错误 
};
~~~

# 条款33：避免遮掩继承而来的名称

**派生类的名称会掩盖基类的名称。在public继承下从没有人希望如此**

**为了让被掩盖的名称重见天日，可用using声明式或者转交函数**

- 查找名称从内而外的，内部查到会遮盖外部名称
- 名称只要相同就会遮蔽，和参数无关

~~~cpp
class Base{
private:
    int x;
public:
    virtual void f1() = 0;
    virtual void f1(int);
    virtual void f2();
    void f3();
    void f4();
};

class Derived:public Base
{
public:
    virtual void f1();
    void f3();
    void f4();
}

Derived d;
int x;

d.f1();
d.f1(x);//error,f1将父类两个同名函数都遮盖了
d.f2();
d.f3();
d.f3(x);//error

//解决方法：
class Derived:public Base
{
public:
    using Base::f1;
    using Base::f3;//将父类函数暴露给子类
    virtual void f1();
    void f3();
    void f4();
}

Derived d;
int x;

d.f1();//调用子类的
d.f1(x);//调用父类的
d.f2();//父
d.f3();//子
d.f3(x);//父
~~~

- 转交函数 处理并不像继承全部base class的情况

~~~cpp
class Base{
private:
    int x;
public:
    virtual void f1() = 0;
    virtual void f1(int);
};

class Derived:private Base
{
public:
    virtual void f1(){Base::f1();}//转交到基类的f1
}

Derived d;
int x;

d.f1();//调用子类的
d.f1(x);//error,Base::f1被遮盖
~~~

# 条款34：区分接口继承和实现继承

**接口继承和实现继承不同。在public继承下，子类总是继承基类的接口**

**纯虚函数只具具体指定接口继承**

**虚函数具体指定接口继承及缺省实现继承**

**普通函数具体指定接口继承以及强制性实现继承**

- public继承包含函数接口继承和函数实现继承

~~~cpp
//实例1
class Shape{
public:
    virtual void draw()const = 0;//纯虚函数的目的是让派生类只继承函数接口，告诉派生类你必须提供一个draw函数，但不管你是如何实现的
    virtual void error(const string&msg);//简朴虚函数的目的是让派生类继承其函数接口和缺省实现。会提供一份实现代码，子类可能覆写它。告诉派生类你必须支持一个error函数，可以自己提供一个，也可以使用缺省版本
    int objectID()const;//非虚函数是为了让子类继承函数的接口及一份强制性实现
};

class Rectangle :public Shape{};
class Ellipse :public Shape{};


//实例2：切断接口和实现的连接
	//实现方法一
class Airplaane{
public:
    virtual void fly(const Airport& destination) = 0;//将fly和缺省的fly分开，用于切断接口和实现的连接
protected:
    void defaultFly(const Airport& destination);
};

class ModelA:public Airplane{
public:
    virtual void fly(const Airport& destination)
    {
        defaultFly(destination);//需要缺省的fly只需要调用它
    }
};
class ModelB:public Airplane{
public:
    virtual void fly(const Airport& destination)
    {
        //自定义fly操作
    }
};
	//实现方法二
class Airplaane{
public:
    virtual void fly(const Airport& destination) = 0;//fly的声明是个接口，fly的定义是个缺省实现（子类明确提出使用才能），用于切断接口和实现的连接
};
//给纯虚函数一个定义
void AirplaneLLfy(const Airplane& destination){//缺省行为}

class ModelA:public Airplane{
public:
    virtual void fly(const Airport& destination)
    {
        Airplane::(destination);//需要缺省的fly只需要调用它
    }
};
class ModelB:public Airplane{
public:
    virtual void fly(const Airport& destination)
    {
        //自定义fly操作
    }
};
~~~

# 条款35：考虑virtual函数以外的其他选择

**virtual函数的替换方法包括NVI（模板方法的一种）和策略模式**

**将机能从函数成员转移到class外部函数，带来一个缺点：非成员函数无法访问class的non-public成员**

**tr1::function对象的行为就像一般的函数指针。这样的对象可接纳“与给定的目标签名式兼容”的所有可调用物**

- 使用non-virtual interface(NVI)手法，是模板方法设计模式的一种特殊形式。它以public non_virtual成员函数包裹较低访问性（private,protected）的virtual函数

- 将virtual函数替换成“函数指针成员变量”，是策略模式的一种分解表现形式

- 以tr1::function成员变量替换virtual函数，因而允许使用任何可调用物搭配一个兼容于需求的签名式。也是策略模式的某种形式

- 将继承体系内的virtual函数替换成另一个继承体系的virtual函数，是策略模式的传统手法

- NVI手法（非虚接口）又称模板方法

~~~cpp
class GameCharacter{
public:
    int healthValue()const//虚函数的外覆器
    {
       	int retVal = doHealthVal();
        return retVAl;
    }
private:
    virtual int doHealthVal()const{}//缺省算法，子类可重新定义继承来的private virtual函数
};
~~~

- 策略模式

~~~cpp
//使用函数指针实现，本例的局限性在于只能使用一个返回int的函数
class GameCharacter;

int defaultHealthCalc(const GameCharacter&gc);

class GameCharacter{
public:
    typedef int (*HealthCalcFunc)(const GameCharacter&);//函数指针代替虚函数
    
    explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc):healthFunc(hcf){}
    
    int healthValue()const{return healthFunc(*this);}
private:
    HealthCalcFunc healthFunc;
};

class EvilBadGuy:public GameCharacter{
public:
    explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc):GameCharacter(hcf){}
}

int loseHealthQuickly(const GameCharacter&);
int loseHealthSlowly(const GameCharacter&);

EvilBadGuy ebg1(loseHealthQuickly);
EvilBadGuy ebg1(loseHealthSlowly);
~~~

~~~cpp
//使用tr1::function实现：本例的参数只要能被隐式转换成const GameCharacter&，返回值能被隐式转换成int即可使用
class GameCharacter;

int defaultHealthCalc(const GameCharacter&gc);

class GameCharacter{
public:
    typedef std::tr1::function<int (const GameCharacter&)>HealthCalcFunc;//产生的对象可以保存任何与此签名式兼容的可调用物，
    
    explicit GameCharacter(HealthCalcFunc hcf = defaultHealthCalc):healthFunc(hcf){}
    
    int healthValue()const{return healthFunc(*this);}
private:
    HealthCalcFunc healthFunc;
};

class EvilBadGuy:public GameCharacter{
public:
    explicit EvilBadGuy(HealthCalcFunc hcf = defaultHealthCalc):GameCharacter(hcf){}
};

//返回short类型
short calcHealth(const GameCharacter&);
//函数对象
struct HealthCalculator{
	int operator()(const GameCharacter&)const{}
};
//类函数
class GameLevel{
public:
    float health(const GameCharacter&)const;
};


EvilBadGuy ebg1(calcHealth);
EvilBadGuy ebg1(HealthCalculator());
GameLevel currentlevel;
EvilBadGuy ebg3(std::tr1::bind(&GameLevel::health,currentLevel,_1));
~~~

# 条款36：绝不重新定义继承而来的non-virtual函数

- 避免遮蔽问题，导致子类不再满足 is a 父类

# 条款37：绝不重新定义继承而来的缺省参数值

**绝对不要重定义一个继承而来的缺省参数值，因为缺省参数值是静态绑定，而虚函数是动态绑定**

- 虚函数动态绑定，缺省数值静态绑定

~~~cpp
class Shape{
public:
    enum shapeColor{Red,Green,Blue};
    virtual void draw(shapeColor color = Red)const = 0;
};
class Rectangle:public Shape{
public:
    virtual void draw(shapeColor color = Green)const;//静态绑定下这个函数不从基类继承缺省参数值，但若以指针调用此函数，可以不指定参数值，动态绑定会继承基类的缺省参数值
};

Shape* ps;//ps静态类型Shape*,无动态类型
Shape* pr = new Rectangle;//pr静态类型Shape*，动态类型Rectangle*

pr->draw(Shape::Red);//调用rectangle的draw
pr->draw();//调用rectangle的draw(Shape::Red)而非Green
ps = pr;//此时ps静态类型Shape*，动态类型Rectangle*
~~~

- NVI替代方法

~~~cpp
class Shape{
public:
    enum ShapeColor{Red,Green,Blue};
    void draw(ShapeColor color = Red)const//由于非虚函数绝对不该被子类覆写，这样设计使得draw的color缺省值总是red
    {
		doDraw(color);
    }
private:
    virtual void doDraw(ShapeColor color)const = 0;
};

class Rectangle:public Shape{
public:

private:
    virtual void doDraw(ShapeColor color)const;
};
~~~

# 条款38：通过复合塑模出has-a或“根据某物实现出”

**复合的意义和public继承完全不同**

**在应用域，复合意味着has-a。在实现域，符合意味着is-implemented-terms-of**

- has-a

~~~cpp
class A;

class B{
private:
    A a;
};
~~~

- is-implemented-in-terms-of根据某物实现出 = has-a + use for implement

~~~cpp
//用list实现set
template<class T>
class Set{
public:
    bool member(const T& item)const;
    void insert(const T& item);
private:
    std::list<T>rep;
}

template<typename T>
bool Set<T>::member(const T& item)const
{
    return std::find(rep.begin(),rep.end(),item)!=rep.end();
}

template<typename T>
void Set<T>::insert(const T& item)
{
    if(!member(item)rep.push_back(item));
}
~~~

# 条款39：明智而审慎使用private继承

**private继承意味着is-implemented-in-terms-of（根据某物实现出）。它通常比复合的级别低。但是当子类需要访问protected base class的成员，或需要重新定义继承而来的virtual函数时，这么设计是合理的**

**和复合不同，private继承可以造成empty base最优化，这对致力于对象尺寸最小化的程序很重要**

- 尽可能使用复合实现“根据某物实现出”，必要时**（当protected和virtual函数牵扯进来）**才用private继承

~~~cpp
//private继承
class Timer{
public:
    explicit Timer(int tickFrequency);
    virtual void onTick()const;
};

class Widget :private Timer
{
private:
	virtual void onTick()const;//private继承将此函数变为private，重写时仍在这    
}

//等价:public继承+复合
class Widget{
private:
    class WidgetTimer : public Timer
    {
    public:
        virtual void onTick()const;
    };
    WidgetTimer timer;
};
~~~

~~~cpp
class Empty{};//size == 1
class HoldEmpty{//size == 8
private:
    int x;//size == 4
    Empty e;//size == 4
};

class PrivateEmpty : private Empty//size == 4
{
private:
    int x;//size == 4
};
~~~

# 条款40：明智而审慎地使用多重继承

**多重继承比单一继承复杂。他可能导致新的歧义性，以及对virtual继承的需要**

**virtual继承会增加大小，速度，初始化复杂度等成本。如果虚基类不带任何数据，将是最具实用价值的情况**

**多重继承的确有正当用途。其中一个情节涉及“public继承某个“Interface class”和private继承某个协助实现的class的两相组合**

~~~cpp
class BaseA
{
public:
    void checkOut();
};

class BaseB
{
private:
    bool checkOut();
};

class derived : public BaseA,public BaseB
{
    
};

derived d;
d.checkOut();//error造成歧义，无法确定调用的checkout时来自哪个基类，尽管B的该函数时private
d.BaseA::checkOut();//明确了
d.BaseB::checkOut()//error 试图调用private
~~~

- 钻石型多重继承:使用**虚继承**防止子类产生同样的东西两份

~~~cpp
class grandfather{};
class father1:virtual pubic grandfather{};
class father2:virtual pubic grandfather{};
class son : public father1,public father2{};
~~~

- 忠告
  - 非必要不适用虚基：开销大，容易错
  - 尽量避免在虚基放数据：防止初始化错误

~~~cpp
class IPerson{//person的接口
public:
    virtual ~IPerson();
    virtual string name() const = 0;
    virtual string birthDate() const = 0;
};
class DatebaseID{};
//shared_ptr<IPerson>makePerson(DatebaseID personIdentifier);//根据ID创建Person对象
DatebaseID askUserForDatebaseID();//获取一个ID
DatebaseID id(askUserForDatebaseID());
shared_ptr<IPerson>pp(makePerson(id));

class PersonInfo{//协助类
public:
    explicit PersonInfo(DatebaseID pid);
    virtual ~PersonInfo();
    virtual const char*theName()const;
    virtual const char* theBirthDate() const;
private:
    virtual const char* valueDelimOpen() const {return "[";}
    virtual const char* valueDelimClose() const {return "]";}
};

class Cperson :public IPerson,private PersonInfo{
public:
    explicit Cperson(Datebase pid):PersonInfo(pid){}
    virtual string name()const
    {
        return PersonInfo::theName();
    }
    
    virtual string birthDate()const
    {
		return PersonInfo::theBirthDate();
    }
private:
    const char* valueDelimOpen()const{return "";}
    const char* valueDelimClose()const {return "";}
};
~~~

# 条款41：了解隐式接口和编译期多态

**class和templates都支持接口和多态**

**对class而言接口是显式的，以函数签名为中心。多态是通过virtual函数发生于运行期**

**对template参数而言，接口是隐式的，奠基于有效表达式。多态则是通过template具现化和函数重载解析发生于编译期**

- 显式接口和运行期多态

~~~cpp
class Widget{
public:
    Widget();
    virtual ~Widget();
    virtual size_t size()const;
    virtual void normalize();
};

void doProcessing(Widget&w)
{
    if(w.size() > 10)
        w.normalize();
}
~~~

- 隐式接口和编译期多态

~~~cpp
template<typename T>//以不同的template参数具现化function templates会导致调用不同函数，这就是编译期多态
void doProcessing(T& w)
{
    if(w.size() > 10)
        w.normalize();
}
~~~

# 条款42：了解typename的双重意义

**声明template参数时，前缀关键字class 和 typename可以互换**

**请使用关键字typename标识嵌套从属类型名称，但不得在base classes list或者member initilization list内以ta作为修饰符**

~~~cpp
template<typename T>
template<class T>
    //完全等价
~~~

- 任何时候当你想在template中指涉一个嵌套从属类型名称，就必须在紧邻它的前一个位置放上关键字typename

~~~Cpp
template<typename C>
void f(const C& container,typename C::iterator iter);
~~~

- typename不可以出现在base classes list内的嵌套从属类型名称之前，也不可以在member initialization list中作为bases class的修饰符

~~~cpp
template<typename T>
class Derived:public Base<T>::Nested{//base classes list
public:
    explicit Derived(int x):Base<T>::Nested(x)//member initialization list
    {
        typename Base<T>::Nested temp;//嵌套从属类型名需要加上typename
    }
}
~~~

~~~cpp
template<typename IterT>
void workWithIterator(IterT iter)
{
    typedef typename std::iterator_traits<IterT>::value_type value_type;
    value_type temp(*iter);
}
~~~

# 条款43：学习处理模板化基类内的名称

**可在derived class templates内通过this->指涉base templates内的成员名称，或可藉由一个明白写出的base class资格修饰符完成**

~~~cpp
class CompanyA{
public:
    void sendCleartext(const string& msg);
    void sendEncrypted(const string& msg);
};

class CompanyB{
public:
    void sendCleartext(const string& msg);
    void sendEncrypted(const string& msg);
};

class CompanyZ{
public:
    void sendEncrypted(const string& msg);
    //不提供sendCleartext
}

class MsgInfo{};

//泛化
template<typename Company>
class MsgSender{
public:
    void sendClear(const MsgInfo& info){
        string msg;
        Company c;
        c.sendCleartext(msg);
    }
    
    void sendSecret(const MsgInfo& info){
        string msg;
        Company c;
        c.sendEncrypted(msg);
    }
};

//全特化
template<>
class MsgSender<CompanyZ>{
public:
    void sendSecret(const MsgInfo& info) {}
};

template<typename Company>
class LoggingMsgSender:public MsgSender<Company>{
public:
    void sendClearMsg(const MsgInfo& info){
        sendClear(info);//调用base class的函数，无法通过编译，因为可能有基类被特化，如果是conmanyZ无此函数
    }
};
~~~

- 进入templatized base classes观察的行为不失效的方法

~~~cpp
//方法1：在基类函数的调用动作前加this->
template<typename Company>
class LoggingMsgSender:public MsgSender<Company>{
public:
    void sendClearMsg(const MsgInfo& info){
        this->sendClear(info);//调用base class的函数，无法通过编译，因为可能有基类被特化，如果是conmanyZ无此函数
    }
};

//方法2：using声明式
template<typename Company>
class LoggingMsgSender:public MsgSender<Company>{
public:
    using MsgSender<Company>::sendClear;
    void sendClearMsg(const MsgInfo& info){
        sendClear(info);//调用base class的函数，无法通过编译，因为可能有基类被特化，如果是conmanyZ无此函数
    }
};

//方法3：明确指出被调用的函数在基类
template<typename Company>
class LoggingMsgSender:public MsgSender<Company>{
public:
    void sendClearMsg(const MsgInfo& info){
        MsgSender<Company>::sendClear(info);//调用base class的函数，无法通过编译，因为可能有基类被特化，如果是conmanyZ无此函数
    }
};
~~~

# *条款44：将与参数无关的代码抽离templates

**Templates生成多个classes和多个函数，所以任意template代码都不该和某个造成膨胀的template参数产生相依关系**

**因非类型模板参数而造成的代码膨胀，往往可以消除，做法是以函数参数或class成员变量替换template参数**

**因类型参数而造成的代码膨胀，往往可降低，做法是让带有完全相同二进制表述的具现型和共享现码**

~~~cpp
template<typename T,size_t n>
class A{
public:
    void insert();
}

A<double,5> sm1;
sm1.insert();
A<double,10>sm2;
sm2.insert();
//具现化两份insert，然而仅有一个数字改变了，发生代码膨胀
~~~

~~~cpp
//解决方法1
template<typename T>
class ABase{
protected:
    void insert(size_t n);
};

template<typename T,size_t n>
class A:private ABase<T>
{
private:
    using ABase<T>::insert;
public:
    void insert(){this->insert(n);}
};

A<double,5> sm1;
sm1.insert();
A<double,10>sm2;
sm2.insert();//此时共享一个base class的insert
~~~

~~~cpp
//解决方法2
template <typename T>
class ABase
{
protected:
    ABase(size_t n,T*pMem):size(n),pData(pMem){}
    void setDataPtr(T* ptr){pData = ptr;}
private:
    size_t size;
    T*pData;
};

template<typename T,size_t n>
class A:private ABase<T>{
public:
    A():ABase<T>(n,data){}
private:
    T data[n*n];
};
~~~

# 条款45：运用成员函数模板接受所有兼容类型

**使用member function templates 生成 “可接受所有兼容类型”**

**如果你声明member templates 用于泛化copy构造或泛化assignment操作，你还需要声明正常的copy构造函数和copy assignment操作符**

~~~cpp
template<typename T>
class smartPtr{
public:
    template<typename U>
    SmartPtr(const SmartPtr<U>& other):heldPtr(other.get()){...}//泛化构造函数,对于任意u创建t对象
    T*get()const{return heldPtr;}
private:
    T*heldPtr;
}
~~~

- shared_ptr实现

~~~cpp
template<Class T>
class shared_ptr{
public:
    template<class Y>
    explicit shared_ptr(Y* p);
    
    template<class Y>
    shared_ptr(shared_ptr<Y>const& r);
    
    template<class Y>
    explicit shared_ptr(weak_ptr<Y>const&r);
    
    template<class Y>
    explicit shared_ptr(auto_ptr<Y>& r);
    
    template<class Y>
    shared_ptr& operator=(shared_ptr<Y>const& r);
    
    template<class Y>
    shared_ptr& operator=(auto_ptr<Y>& r);
};
~~~

# 条款46：需要类型转换时请为模板定义非成员函数

**当我们编写一个class templates ，而他提供的“与此template相关的”函数支持“所有参数的隐式转换”时，请将那些函数定义为“class template内部的friend”**

~~~cpp
template<typename T>
class Rational{
public:
    friend const Rational operator*(const Rational&lhs,const Rational&rhs);//类型转换不能是成员函数，自动具现化需要声明在class内部，所以声明为友元
};

template<typename T>
const Rational<T>*operator*(const Rational<T>&lhs,const Rational<T>&rhs)
{
	return Rational(lhs.num()*rhs.num());
}
~~~

# 条款47：请使用traits class 表现类型信息

**Traits classes使得“类型相关信息”在编译器可用。他们以templates和templates 特化完成实现**

**整合重载技术，traits classes有可能在编译期对类型执行if else 测试**

- input迭代器只能向前移动，一次一步读取所指内容。模仿阅读指针

- output迭代器类似，模拟读写指针
- forward迭代器：只能向前移动
- bidirectional：可以双向移动
- random access迭代器：可在常量时间向前或向后跳跃任意距离

~~~cpp
struct input_iterator_tag{};
struct output_iterator_tag{};
struct forward_iterator_tag:public input_iterator_tag{};
struct bidirectional_iterator_tag:public forward_iterator_tag{};
struct random_access_iterator_tag:public bidirectional_iterator_tag{};
~~~

- iterator_traits

~~~cpp
template<typename IterT>
struct iterator_traits{
    typedef typename IterT::iterator_category iterator_category;
};

template<typename IterT>//针对内部指针偏特化
struct iterator_traits<IterT*>{
    typedef random_access_iterator_tag iterator_category;
};

template<...>
class list{
pubic:
    class iterator{
    public:
        typedef bidirectional_iterator_tag iterator_category;//设定一种迭代器类型
    };
    ...
};
~~~

- advance函数：用来将迭代器移动给定距离（TMP案例）

~~~cpp
//重载不同tag版本
template<typename IterT,typename DistT>
void doAdvance(IterT& iter ,DistT d,std::random_access_iterator_tag)
{
    iter += d;
}

template<typename IterT,typename DistT>
void doAdvance(IterT& iter ,DistT d,std::bidirectional_iterator_tag)
{
    if( d >= 0){while(d--)++iter;}
    else {while(d++)--iter;}
}

template<typename IterT,typename DistT>
void doAdvance(IterT& iter ,DistT d,std::input_iterator_tag)//forward_iterator_tag继承自input_iterator_tag，也可用该版本
{
    if( d < 0){throw std::out_of_range("Negative distance");}
    while(d--)--iter;
}

template<typename IterT , typename DistT>
void advance(IterT& iter , DistT d)//d<0向后移动
{
    doAdvance(iter,d,typename std::iterator_traits<IterT>::iterator_category());//将ifelse由doadvance的特化体表现
}
~~~

# 条款48：认识template元编程（TMP）

**TMP可将工作由运行期移往编译期，因而获得更高的效率和得以早期侦错**

**TMP可被用来生成“基于政策选择组合”的客户端定制代码，也可用来避免生成对某些特殊类型并不适合的代码**

- 作用
  - 让某些事情更容易。没有他的话有些事情将是几乎不可能完成的
  - TMP执行于编译期，有利于在编译期就发现错误，生成的程序更高效，内存需求小。但是编译时间变长

# 条款49：了解new-handler的行为

**set_new_handler允许客户指定一个函数，在内存分配无法得到满足时调用**

**Nothrow是一个颇为局限的工具，仅适用于内存分配；后继的构造函数调用还可能抛出异常**

- new-hander用于处理new内存不足

~~~cpp
namespace std{
    typedef void(*new_handler)();
    new_handler set_new_handler(new_handler p) throw();
}

void OutOfMem()
{
	std::cerr<<"Unable to satisfy request for memory\n";
    std::abort();
}

int main()
{
	std::set_new_handler(OutOfMem);
    int* pBigDataArray = new int[100000000L];//分配失败调用OutOfMem
}
~~~

- 类专属的new-handler

~~~cpp
class Widget{
public:
    static std::new_handler set_new_handler(std::new_handler p)throw();
    static void* operator new(std::size_t size)throw(std::bad_alloc);
private:
    static std::new_handler currentHandler;
}

std::new_handler currentHandler = 0;

std::new_handler Widget::set_new_handler(std::new_handler p)throw()
{
	std::new_handler oldHandler = currentHandler;
    currentHandler = p;
    return oldHandler;
}
~~~

~~~cpp
class NewHandlerHolder{
public:
    explicit NewHandlerHolder(std::new_handler nh):handler(nh){}//获取目前的handler
    ~NewHandlerHolder(){std::set_new_handler(handler);}//释放它
    
private:
    std::new_handler handler;
    NewHandlerHolder(const NewHandlerHolder&);
    NewHandlerHolder&operator=(const NewHandlerHolder&);
};

void* Widget::operator new(std::size_t size)throw(std::bad_alloc)
{
    NewHandlerHolder h(std::set_new_handler(currentHandler));
    return ::operator new(size);
}

void outOfMem();//Widget对象分配失败时调用
Widget::set_new_handler(outOfMem);//设置handler函数
Widget* pw1 = new Widget;//分配内存，失败调用outOfMem
std::string* ps = new std::string;//分配内存失败调用global handler函数
Widget::set_new_handler(0);//设定handler为null
Widget* pw2 = new Widget;//内存分配异常，立刻抛出异常（无专属的handler）
~~~

~~~cpp
template<typename T>
class NewHandlerSupport{
public:
    static std::new_handler set_new_handler(std::new_handler p)throw();
    static void* operator new(std::size_t size)throw(std::bad_alloc);
    
private:
    static std::new_handler currentHandler;
};

template<typename T>
std::new_handler NewHandlerSupport<T>::set_new_handler(std::new_handler p)throw()
{
	std::new_handler oldHandler = currentHandler;
    currentHandler = p;
    return oldHandler;
}

template<typename T>
void* NewHandlerSupport<T>::operator new(std::size_t size)throw(std::bad_alloc)
{
    NewHandlerHolder h(std::set_new_handler(currentHandler));
    return ::operator new(size);
}
template<typename T>
std::new_handler NewHandlerSupport<T>::currentHandler = 0;//初始化

class Widget:public NewHandlerSupport<Widget>{};
~~~

~~~cpp
//分配失败返回null的行为称为Nothrow
class Widget{};
Widget*pw1 = new Widget;//分配失败返回bad_alloc
if(pw == 0)...//这个测试一定失败
    
Widget* pw2 = new(std::nothrow)Widget;//分配失败返回0
if(pw2 == 0)...//这个测试可能成功
~~~

# 条款50：了解new和delete的合理替换时机

**写个自定义的new和delete有很多理由，包括改善效能，对heap运用错误进行调试，收集heap使用信息**

- 为了检测运用错误
  - 包括new之后没delete；new之后多次delete；地址在选区的范围之前或之后
- 为了强化效能：考虑破碎问题，大块内存被取用归还成零散的小内存
- 为了收集动态分配内存之使用统计信息：如何分配的内存，什么次序分配和归还，运用形态是否随时间改变
- 为了增加分配和归还速度：泛用型分配器有线程安全检测，做一个单线程不需要线程安全检测，所以可以节省时间
- 为了降低缺省内存管理器带来的空间额外开销：专用的分配器往往不会有多余的操作占内存
- 为了弥补缺省分配器中的非最佳齐位：泛用型不保证是最佳对齐方式
- 为了将关联对象成簇集中：将相关对象放在一个heap上，集中在尽可能少的页上，降低内存页错误
- 为了获取非传统行为：实现泛用型没有的功能

# 条款51：编写new和delete时需固守常态

**operator new 应该含有一个无穷循环，并在其中尝试分配内存，如果他无法满足内存需求，就调用new-handler。它也应该有能力处理0bytes申请。class专属版本应该处理比正确大小更大的错误**

**operator delete应该在收到null指针时不做任何事。class专属版本还应处理比正确大小更大的错误**

- new

~~~cpp
class Base{
public:
	static void* operator new(std::size_t size)throw(std::bad_alloc);
};

void* Base::operator new(std::size_t size)throw(std::bad_alloc)
{
    //if(size == 0) size = 1;//不是类函数需要这一句，分配空内存也返回一个指针
    if(size != sizeof(Base))//类的大小至少1，上一句不需要
        return ::operator new(size);//如果不是基类（可能是派生类）令标准operator new处理
    using namespace std;
    while(true)
    {
        分配字节;
    	if(分配成功)
            return 指向内存的指针;
        
        new_handler globalHandler = set_new_handler(0);
        set_new_handler(globalHandler);
        
        if(globalHandler)(*globalHandler)();
        else throw std::bad_alloc();
    }
    
}

class Derived:public Base{};

Derived* p = new Derived;

~~~

- delete

~~~cpp
class Base{
public:
	static void operator delete(void* rawMemory,std::size_t size)throw();
};

void Base::operator delete(void*rawMemory,std::size_t size)throw()
{
    if(rawMemory == 0)return ;
    if(size != sizeof(Base))
    {
        ::operator delete(rawMemory);
        return;
    }
    归还rawMemory所指的内存;
    return;
}
~~~

# 条款52：写了placement new 也要写placement delete

**当你写了一个placement operator new，请确定也写出了对应的placement operator delete。如果没有这样做，你的程序可能会发生隐微而时断时续的内存泄漏**

**当你声明placement new和placement delete，请确定不要无意识地掩盖他们的正常版本**

- new：在空内存分配
- operator new ：重载new
- placement new：在已有的内存上分配，参数除了size_t还有其他的参数

~~~cpp
void* operator new(std::size_t,void* pMemory)throw();
~~~

- 如果内存分配成功，但是构造函数发生异常，系统有责任取消operator new的分配并恢复旧观

~~~cpp
class Widget{
public:
    static void* operator new(std::size_t size,std::ostream&logStream)throw(std::bad_alloc);
    static void operator delete(void* pMemory)throw();//正常版new
	static void operator delete(void* pMemory,std::ostream& logStream)throw();//placement delete
    //如果少以上某一个版本，就会被另一个版本覆盖，不会默认生成
};

Widget* pw = new(std::cerr)Widget;//有了placement new，这次不再内存泄露

delete pw;//调用正常的delete
~~~

~~~cpp
void* operator new(std::size_t)throw(std::bad_alloc);//normal new
void* operator new(std::size_t,void*)throw();//placement new
void* operator new(std::size_t,const std::nothrow_t&)throw();//nothrow new
~~~

- 为了避免operator news，operator deletes覆盖标准形式，可以声明一个Base class

~~~cpp
class StandardNewDeleteForms
{
public:
    static void* operator new(std::size_t)throw(std::bad_alloc){return ::operator new(size);}//normal new
    static void operator delete(void* pMemory)throw(){::operator delete(pMemory);}//normal new
    
	static void* operator new(std::size_t,void*ptr)throw(){return ::operator new (size,ptr);}//placement new
    static void operator delete(void* pMemory,void*ptr)throw(){return ::operator delete(pMemory,ptr);}//placement delete
    
	static void* operator new(std::size_t,const std::nothrow_t& nt)throw(){return operator::new(size,nt);}//nothrow new
    static void* operator new(void* pMemory,const std::nothrow_t& nt)throw(){return operator::delete(pMemory);}//nothrow delete
};
~~~

# 条款53：不要轻忽编译器的警告

**严肃对待编译器发出的任何警告信息。努力在你的编译器地最高警告级别下争取“无任何警告荣誉”**

**不要过度依赖编译器的报警能力，因为不同的编译器对待事情的态度并不相同。一旦移植到另一台编译器上，原本的警告就有可能消失**

# 条款54：让自己熟悉包括TR1在内的标准程序库

**C++标准库由STL，iostreams，locales组成。并包含C99标准程序库**

**TR1添加了智能指针，一般化函数指针，hash—based容器，正则表达式等**

**TR1自身知识一份规范。为获得TR1提供的好处，需要一份实物。一个好的实物来源是Boost**

- 智能指针
  - shared_ptr
  - weak_ptr

- tr1::function
  - 表示任何可调用物，即函数或者函数对象

~~~cpp
void registerCallback(std::string func(int));
void registerCallback(std::string (int));//equal to upper

void registerCallback(std::tr1::function<std::string(int)>func);//只要函数签名满足即可
~~~

- tr1::bind
  - 能够做STL的绑定器
  - 可以和const，非const成员函数协作运作
  - 可以和by-reference参数协同运作

- hash tables
  - 用来实现tr1::unordered_set，tr1::unordered_map...
  - 无次序

- 正则表达式
  - 字符串查找替换匹配迭代...
- Tuples（变量组）
  - pair的替代品，pair只能两个对象，tuple可以任意个对象
- tr1::array
  - 支持begin，end的数组，大小固定
- tr1::mem_fn
  - 扩充了mem_fun,mem_fun_ref

- tr1::reference_wrapper
  - 一个让reference的行为更像对象的设施，可以造成容器“犹如持有reference”

- 随机数生成工具
- 数学特殊函数
- C99兼容扩充
- Type traits
  - 用来提供类型的编译器信息
- tr1::result_of
  - 推导函数调用的返回类型

# 条款55：让自己熟悉Boost

**Boost是一个社群，也是一个网站。致力于C++程序库开发。**

**Boost提供很多TR1组件实现品，以及许多其他程序库**

- 字符串与文本处理
  - 覆盖具备类型安全的printf-like格式化动作，正则表达式，以及语汇单元切割和解析

- 容器
  - 覅该接口和STL相似且大小固定的数组，大小可变的bitsets以及多维数组
- 函数对象和高级编程
  - Lambda
- 泛型编程
  - 覆盖一大组traits classes
- 模板元编程
  - 针对编译期assertions而写的程序库
  - Boost MPL程序库
- 数学和数值
  - 有理数，八元数，四元数，公约数，多重运算，随机数

- 正确性与测试

  - 覆盖将隐式接口形式化的程序库

  - 针对“测试优先”编程形态而设计的措施

- 数据结构

  - 覆盖类型安全的unions，以及tulpe程序库

- 语言间的支持

  - c++和python的无缝互操作性

- 内存

  - 覆盖Pool程序库，用来做高效而区块大小固定的分配器，
  - non-TR1智能指针 scoped_array，用来动态分配数组

- 杂项

  - CRC检验，日期和时间吃力，再文件系统来回移动
