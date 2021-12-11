- `.\`表示当前目录

- `echo`获取main命令的返回值

- `iostream`标准库的IO机制，包含输入流`istream`，输出流`ostream`

- `cerr` 标准错误，输出警告和错误信息

- `clog` 输出程序运行时的一般性信息

- `::`作用域运算符

- 读取数量不定的输入数据`while(std::cin>>val) sum+=val;`
- `()`调用成员函数(方法)，`if(item1.isEqual() == item2.isEqual())`

- 转义序列

	\n	换行符		 	   \'  单引号
	\t	横向制表符		  \"  双引号
	\a	响铃				\f	进纸符
	\v	纵向制表符		  \?	问号
	\\	反斜线				\b	退格符
	\r	回车符

- `extern`只想声明而不定义使用，且不要显式初始化。可以给extern显式初始化，一旦这样做，就成了定义。在函数体内部初始化extern标记的变量，将引发错误

~~~cpp
extern int i; //声明
int j;        //声明并定义
extern double pi-3.1416 //定义
~~~

- void* 指针是一种特殊的指针类型，可存放任意类型对象的地址

void* 指针能做的事：与其他指针比较大小  作为函数的输入输出传递  赋值给另一个void *指针

void* 指针不能直接操作void*指针所指的对象，因为不知对象类型

- const

  默认状态下，const对象仅在文件内有效

在编译期将const变量都替换为对应的值

当多个文件出现了同名的const变量时，等同于在不同文件中分别定义了独立的变量。

如果要在文件间共享const对象，就必须在声明和定义中都用extern，即将所有该对象都声明为文件外可见。

~~~cpp
// .h
extern const int a;

// .cpp
extern const int a = 0;
~~~

const的引用，对常量的引用不可修改其绑定的对象。

~~~cpp
const int ci = 1024;
const int &r1 = ci;
r1 = 2048;//error，r1是对常量的引用
int &r2 = ci;//error,非常量引用指向一个常量引用
~~~

引用的类型必须严格匹配

若不匹配，编译器会如下处理

~~~cpp
double a = 10.3;
const int &b = a;

//编译器执行过程
//const int c = a;
//const int&b = c;
~~~

可将常量引用绑定到非常量对象。此时对象仍可修改，只是不能被这个引用修改。

~~~cpp
int i = 42;
int &r1 = i; 
const int& r2 = i;
std::cout << r2 << std::endl; //输出42
r1 = 0;
std::cout << r2 << std::endl; //输出0
i = 10;
std::cout << r2 << std::endl; //输出10
~~~

- 指向常量的指针&常量指针

~~~cpp
//定义“常量指针”，存放的地址不能修改，但其所指对象的值可以修改
int errNumb=0;
int *const curErr=&errNumb;   //常量指针。curErr是const，是指针，指向int对象
//定义“指向常量的指针”和“指向常量的常量指针”
const double pi=3.14;
const double *cptr=&pi;       //指向常量的指针。cptr是指针，指向const double对象
const double *const pip=&pi;  //指向常量对象的常量指针。pip是const，是指针，指向const double对象

~~~

- 顶层const && 底层const

顶层const是指const指针本身是个常量：常量指针，拷贝是不受影响

底层const是指const指的对象是个常量：指向常量的指针，拷贝时底层类型严格匹配，或者能相互转换。

- constexpr && 常量表达式

常量表达式：指不会改变，且在编译期就能得到结果的表达式（两个条件缺一不可）

用常量表达式初始化的const对象也是常量表达式。

~~~cpp
const int max_files=20; //是常量表达式
const int limit =max_files+1;//是常量表达式
int staff_size=27; //不是，因为类型不是const int
const int sz =get_size(); //不是，因为get_size()运行时才确定
~~~

constexpr：由编译器来验证变量是否是一个常量表达式

~~~cpp
constexpr int mf=20;  //是常量表达式
constexpr int limit=mf+1;//是常量表达式
constexpr int sz=size();//只有当函数size()是constexpr函数时，才是初始化constexpr变量
~~~

- 字面值类型

算术类型、引用和指针属于字面值类型

自定义类Sales_item、IO库、string类型不属于字面值类型

- 类型别名

定义方法：

旧版：typedef

新版：using

~~~cpp
typedef double wages;   //wages是double的同义词
typedef wages base, *p;  //base是double的同义词，p是double *的同义词

using SI=Sales_item;    //SI是Sales_item的同义词
~~~

