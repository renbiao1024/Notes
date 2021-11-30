```c++
// TArray元素为数值类型时，被销毁时其中的元素也将被销毁。

// 若在另一TArray中创建TArray变量，其元素将复制到新变量中，且不会共享状态。

// 创建

TArray<int32> IntArray;

//填充多个相同元素

IntArray.Init(10,5);//[10,10,10,10,10]

//添加

//可添加重复元素，添加时创建临时变量再复制

IntArray.Add(12);//[10,10,10,10,10,12]

//不可添加重复元素

IntArray.AddUnique(10);//添加失败 //[10,10,10,10,10,12]

IntArray.AddUnique(11);//添加成功 //[10,10,10,10,10,12,11]

//添加时不创建临时变量

IntArray.Emplace(23); //[10,10,10,10,10,12,11,23]

//将其他TArrat的元素，一次性添加进来

TArray<int32> AddArray;

AddArray.Init(2,3);

IntArray.Append(AddArry,ARRAY_COUNT(AddArray));//[10,10,10,10,10,12,11,23,2,2,2]

//在指定位置插入元素后元素数组的副本

IntArray.Insert(5,4);//[10,10,10,10,5,10,12,11,23,2,2,2]

//设置数组元素的数量

//如新数量大于当前数量，则使用元素类型的默认构造函数新建元素

//如新数量小于当前数量，SetNum 将移除元素。

IntArray.SetNum(14);//[10,10,10,10,5,10,12,11,23,2,2,2,0,0]

IntArray.SetNum(3);//[10,10,10]

//迭代

//方式1

int32 res = 0;

for(auto& i:IntArray)

{

  res += i;

}

//方式2

for(int32 i = 0;i!=IntArray.Num();++i) 

{

  res += IntArray[i];

}

//方式三：迭代器

//函数 CreateIterator 和 CreateConstIterator 可分别用于元素的读写和只读访问

for(auto It = IntArray.CreateConstIterator();It;++It) 

{

  res += *It;

}

//排序，自定义排序

IntArray.Sort();

IntArray.Sort([](const int32 &A,const int32 &B))

{

  return A.len>B.len();

}

//堆排序,也可自定义

IntArray.HeapSort();

//归并排序,也可自定义

IntArray.StableSort();

//查询

//查询数量

int32 Count = IntArray.Num();

//返回指向元素的指针，该操作直接访问数组内存。

int32 *IntPtr = IntArray.GetData();

//获取单个元素的大小

uint32 ElementSize = IntArray.GetTypeSize();

//索引运算符获取元素，返回的是一个引用，可用于操作数组中的元素

int32 Elem1 = IntArray[1];

//确定特定索引是否有效（0≤=索引<Num()）

bool bValid_5 = IntArray.IsValidIndex(5);

//从数组末端反向索引，索引默认为零

int32 ElemEnd = IntArray.Last();// == int32 ElemEnd = IntArray.Last(0);

int32 ElemEnd = IntArray.Last(1);//从后往前数一个

//返回最后一个元素，不接受索引

int32 IntArray.Top();

//查询是否包含特定元素

bool b12 = IntArray.Contains(12);

TArray<FString>StrArr

//查询是否包含与特定谓词匹配的元素

bool bLen5 = StrArr.ContainsByPredicate([](const FString& Str){

  return Str.Len() == 5;

});

//确定元素是否存在并返回找到的首个元素的索引

int32 Index2 = StrArr.Find(TEXT("Hello"));  // Index2 == 3

//确定元素是否存在并返回找到的最末元素的索引

int32 IndexLast2 = StrArr.FindLast(TEXT("Hello")); // IndexLast2 == 3

//返回首个匹配到的元素的索引；如果没有找到元素，则返回INDEX_NONE,允许元素与任意对象进行对比

int32 Index = StrArr.IndexOfByKey(TEXT("Hello")); // Index == 3

//查找与特定谓词匹配的首个元素的索引；如未找到，同样返回特殊 INDEX_NONE 值

int32 Index = StrArr.IndexOfByPredicate([](const FString& Str){

  return Str.Contains(TEXT("r"));

}); // Index == 2

//将元素和任意对象对比，并返回首个匹配到的元素的指针，如果未匹配到，则返回nullptr

auto* OfPtr = StrArr.FindByKey(TEXT("of"))); // OfPtr == &StrArr[1]

//用于查找与特定谓词匹配的首个元素的指针，如果未匹配到，则返回nullptr

auto* Len5Ptr = StrArr.FindByPredicate([](const FString& Str){

  return Str.Len() == 5;

}); // Len5Ptr == &StrArr[2]

//获取与特定谓词匹配的元素数组

auto Filter = StrArray.FilterByPredicate([](const FString& Str){

  return !Str.IsEmpty() && Str[0] < TEXT('M');

});

//移除

//移除数组中的元素

TArray<int32> ValArr;

int32 Temp[] = { 10, 20, 30, 5, 10, 15, 20, 25, 30 };

ValArr.Append(Temp, ARRAY_COUNT(Temp)); //==>[10,20,30,5,10,15,20,25,30]

ValArr.Remove(20); //==>[10,30,5,10,15,25,30]

//用于擦除数组中的首个匹配元素。

ValArr.RemoveSingle(30); //==>[10,5,10,15,25,30]

//用于按照从零开始的索引移除元素。

ValArr.RemoveAt(2); // 移除下标为2的元素, ==>[10,5,15,25,30]

//用于函数移除与谓词匹配的元素

ValArr.RemoveAll([](int32 Val) {

  return Val % 3 == 0; }); //移除为3倍数的所有数值, ==> [10,5,25]

//移动过程存在开销。如不需要剩余元素排序，可使用 RemoveSwap、RemoveAtSwap 和 RemoveAllSwap 函数减少此开销。

TArray<int32> ValArr2;

for (int32 i = 0; i != 10; ++i)

  ValArr2.Add(i % 5); //==>[0,1,2,3,4,0,1,2,3,4]

ValArr2.RemoveSwap(2); //==>[0,1,4,3,4,0,1,3]

ValArr2.RemoveAtSwap(1); //==>[0,3,4,3,4,0,1]

ValArr2.RemoveAllSwap([](int32 Val) {

  return Val % 3 == 0; }); //==>[1,4,4]

//移除数组中所有元素,释放内存

ValArr2.Empty(); //==>[]

//移除数组中所有元素,不释放内存

ValArr2.Reset (); //==>[]

//运算符
//数组得复制是深拷贝
TArray<int32>ValArr3;
ValArr3.Add(1);
ValArr3.Add(2);
ValArr3.Add(3);
auto ValArr4 = ValArr3; // ValArr4 == [1,2,3];
ValArr4[0] = 5;         // ValArr4 == [5,2,3]; ValArr3 == [1,2,3];

//+=  等价于append
ValArr4+=ValArr3;//==>[5,2,3,1,2,3]

//MoveTemp 转移数组（原数组清空）
ValArr3 = MoveTemp(ValArr4);  // ValArr3 == [5,2,3,1,2,3]; ValArr4 == []

//== 两个数组里得所有元素相同且排序相同才返回true，但相同字母得大小写被视为相同得
TArray<FString> FlavorArr1;
FlavorArr1.Emplace(TEXT("Chocolate"));
FlavorArr1.Emplace(TEXT("Vanilla")); // FlavorArr1 == ["Chocolate","Vanilla"]

auto FlavorArr2 = FlavorArr1;        // FlavorArr2 == ["Chocolate","Vanilla"]

bool bComparison1 = FlavorArr1 == FlavorArr2;  // bComparison1 == true

for ( auto& Str : FlavorArr2 )
{
	Str = Str.ToUpper();
} // FlavorArr2 == ["CHOCOLATE","VANILLA"]

bool bComparison2 = FlavorArr1 == FlavorArr2; // bComparison2 == true，因为FString的对比忽略大小写

Exchange(FlavorArr2[0], FlavorArr2[1]); // FlavorArr2 == ["VANILLA","CHOCOLATE"]

bool bComparison3 = FlavorArr1 == FlavorArr2; // bComparison3 == false，因为两个数组内的元素顺序不同

//堆：TArray 拥有支持二叉堆数据结构的函数。堆是一种二叉树，其中父节点的排序等于或高于其子节点。作为数组实现时，树的根节点位于元素0，索引N处节点的左右子节点的指数分别为2N+1和2N+2。子节点彼此间不存在特定排序。
//Heapify 函数可将现有数组转换为堆。
TArray<int32> HeapArr;
for (int32 Val = 10; Val != 0; --Val){
	HeapArr.Add(Val);
} // HeapArr == [10,9,8,7,6,5,4,3,2,1]

HeapArr.Heapify(); // HeapArr == [1,2,4,3,6,5,8,10,7,9]
//HeapPush 函数可将新元素添加到堆，对其他节点进行重新排序，以对堆进行维护
HeapArr.HeapPush(4); // HeapArr == [1,2,4,3,4,5,8,10,7,9,6]
//HeapPop 和 HeapPopDiscard 函数用于移除堆的顶部节点。
//这两个函数的区别在于前者引用元素的类型来返回顶部元素的副本，而后者只是简单地移除顶部节点，不进行任何形式的返回。两个函数得出的数组变更一致，重新正确排序其他元素可对堆进行维护
int32 TopNode;
HeapArr.HeapPop(TopNode); // TopNode == 1; HeapArr == [2,3,4,6,4,5,8,10,7,9]
//HeapRemoveAt 将删除数组中给定索引处的元素，然后重新排列元素，对堆进行维护
HeapArr.HeapRemoveAt(1); // HeapArr == [2,4,4,6,9,5,8,10,7]
//HeapTop 函数可查看堆的顶部节点，无需变更数组
int32 Top = HeapArr.HeapTop();  // Top == 2

//Slack:数组中未被利用得空间
//默认构建的数组不分配内存，slack初始为0。

//GetSlack 函数即可找出数组中的slack量，相当于Max() - Num()

//Max 函数可获取到容器重新分配之前数组可保存的最大元素数量。

//分配器确定重新分配后容器中的Slack量。因此 Slack 不是常量。
TArray<int32> SlackArray;
// SlackArray.GetSlack() == 0
// SlackArray.Num()      == 0
// SlackArray.Max()      == 0

SlackArray.Add(1);
// SlackArray.GetSlack() == 3
// SlackArray.Num()      == 1
// SlackArray.Max()      == 4

SlackArray.Add(2);
SlackArray.Add(3);
SlackArray.Add(4);
SlackArray.Add(5);
// SlackArray.GetSlack() == 17
// SlackArray.Num()      == 5
// SlackArray.Max()      == 22

//虽然无需管理Slack，但可管理Slack对数组进行优化，以满足需求。
//例如，如需要向数组添加大约100个新元素，则可在添加前确保拥有可至少存储100个新元素的Slack，以便添加新元素时无需分配内存。上文所述的 Empty 函数接受可选Slack参数
SlackArray.Empty();
// SlackArray.GetSlack() == 0
// SlackArray.Num()      == 0
// SlackArray.Max()      == 0
SlackArray.Empty(3);
// SlackArray.GetSlack() == 3
// SlackArray.Num()      == 0
// SlackArray.Max()      == 3
SlackArray.Add(1);
SlackArray.Add(2);
SlackArray.Add(3);
// SlackArray.GetSlack() == 0
// SlackArray.Num()      == 3
// SlackArray.Max()      == 3
//Reset 函数与Empty函数类似，不同之处是若当前内存分配已提供请求的Slack，该函数将不释放内存。但若请求的Slack较大，其将分配更多内存：

SlackArray.Reset(0);
// SlackArray.GetSlack() == 3
// SlackArray.Num()      == 0
// SlackArray.Max()      == 3
SlackArray.Reset(10);
// SlackArray.GetSlack() == 10
// SlackArray.Num()      == 0
// SlackArray.Max()      == 10
//Shrink 函数可移除所有Slack。此函数将把分配重新调整为所需要的大小，使其保存当前的元素序列，而无需实际移动元素

SlackArray.Add(5);
SlackArray.Add(10);
SlackArray.Add(15);
SlackArray.Add(20);
// SlackArray.GetSlack() == 6
// SlackArray.Num()      == 4
// SlackArray.Max()      == 10
SlackArray.Shrink();
// SlackArray.GetSlack() == 0
// SlackArray.Num()      == 4
// SlackArray.Max()      == 4

```

