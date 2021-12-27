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

- 用+拼接两个string对象

~~~cpp
string s1 = "Hello ", s2 = "World";
string s3 = s1 + "World";//right
String s4 = "HEllo "+"World";//error
//因为string 和 字面值时不同的类型
~~~

- decltype推测类型

~~~cpp
for(decltype(s.size())index = 0;index!=s.size()&&!isspace(s[index];index++)
    s[index] = toupper(s[index]);
~~~

- 迭代器

~~~cpp
*iter//返回iter所指得元素引用
~~~

- 连续赋值

~~~cpp
int x, y, z;
x = y = z = 1;
//顺序从右到左，z=1, y=z=, x=y
~~~



- sizeof对类型名必须用括号，变量名可以省略

  ~~~cpp
  int n_max = INT_MAX;
  cout<<sizeof(int)
      <<" "<<sizeof(n_max)
      <<sizeof n_max;
  	//<<sizeof int //error
  ~~~

- cout.put()

~~~cpp
//用于显示一个字符
char ch = 77;
//char ch = 'M';
cout.put(ch);
cout.put('!');
~~~

- char

~~~cpp
char ch;//maybe unsigned or signed
unsigned char uch;//0~255
signed char sch;//-128~127
~~~

- wchar_t宽字符类型

wchar_t 16位字符型

wchar_t常用于表示扩展字符集

char表示基本字符集

~~~cpp
wchar_t bob = L'p';//L表示宽字符常量，宽字符串
wcout<<L"tall"<<endl;/
~~~

- const比#define好

1. const 指定了常量的类型
2. const可以使用c++的作用域将常量限定在某个函数or文件内
3. const可用于更复杂的类型

- cout.setf

 ios_base::fixed 表示：用正常的记数方法显示浮点数 (与科学计数法相对应)； ios_base::floatfield 表示小数点后保留6位小数。

~~~cpp
cout.setf(ios_base::fixed,ios_base::floatfield);
~~~

- 数据类型转化

~~~cpp
	int y = int(1.66);//c++把int() 当作类型转换的函数
	y = (int)1.55;//c的写法
	y = static_cast<int>(1.44);//上面的转换有危险
~~~

- 数组的初始化

~~~cpp
int s1[3] = {1,2,3};

int s2[3];
s2[3] = {1,2,3};//error
s2[0] = 1;
s2[1] = 2;
s2[2] = 3;

int s3[] = {1,2,3}//自动推测个数为3
int nums = sizeof s3 / sizeof(in)

int s4[3] = {1};//第一个赋值为1，其余为0

int s5[3] = {0};//全部赋值为0
~~~

- 数组中的string

~~~cpp
const int Size = 15;
char name[Size] = "RenBiao";
cout<<name<<endl;//RenBiao
int nums = strlen(name);
cout<<nums;//7
name[4] = '\0';//遇到\0认为字符串结束
cout<<name<<endl;//RenB
nums = strlen(name);
cout<<nums;//4
~~~

- 读取一行输入

~~~cpp
cin.getline(name,Size); //到换行符结束，并用空字符替换换行符

cin.getline(name1,Size).getline(name2,Size);//把连续两行分别给name1，name2

cin.get(name,Size);  //到换行符结束，但不丢弃换行符，下次输入仍然停在这里
cin,get()//换行,否则一直停留在上一个的换行符处
    
cin.get(name,Size).get();//和上面等价
~~~

- string 的操作

~~~coo
//c风格
strcpy(s3,s1);//复制
strcpat(s3,s2);//末尾附加
//等价于
s3 = s1 + s2;

strlen(s1);

s1.size()
~~~

