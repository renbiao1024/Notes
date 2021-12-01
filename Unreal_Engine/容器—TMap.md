将数据存储为键值对 (TPair<KeyType,ValueType>)

映射有两种类型：

- TMap：键值唯一
- TMultiMap：可以有多个相同键值

## 创建

```c++
TMap<int32,FString> FruitMap;
```

## 添加

### Add

```c++
/*
元素按插入顺序排列，但不保证这些元素在内存中实际保留此排序
各个键都必定是唯一。如果尝试添加重复键,将替换原来的键值
Add 函数可接受不带值的键。调用此重载后的 Add 时，值将被默认构建
*/
FruitMap.Add(5,TEXT("Banana");
FruitMap.Add(4);
// FruitMap == [
//  { Key:5, Value:"Banana"    },
//  { Key:4, Value:""          }
// ]
```

### Emplace 

可以代替 Add，防止插入映射时创建临时文件

```c++
FruitMap.Emplace(3,TEXT("Orange"));
```

### Append

合并映射，将一个映射的所有元素移至另一个映射，源映射的相同键会替代目标映射中的键

```

TMap<int32, FString> FruitMap2;
FruitMap2.Emplace(4, TEXT("Kiwi"));
FruitMap2.Emplace(9, TEXT("Melon"));

FruitMap.Append(FruitMap2);
// FruitMap == [
//  { Key:5, Value:"Mango"     },//替换掉
//  { Key:4, Value:"Kiwi"      },//添加
//  { Key:3, Value:"Orange"    },
//  { Key:9, Value:"Melon"     }
// ]
// FruitMap2 is now empty.
```

## 迭代

### for

```
for(auto& elem:FruitMap) {
	
}
```

### 迭代器

```
for(auto It=FruitMap.CreateConstIterator();It;++It) {
	//It.Key();
	//*It.Value();
}
```

## 查询

### Num

查询映射中保存的元素数量

```
int32 Count = FruitMap.Num();// Count == 4
```

### Contains

查询是否包含特定键

```
bool bHas4 = FruitMap.Contains(4); // bHas4 == true
bool bHas8 = FruitMap.Contains(8); // bHas8 == false
```

### []

将键用作索引查找相应值

- 使用非常量映射执行该操作将返回非常量引用，使用常量映射将返回常量引用。
- 在使用 运算符[] 前，应检查映射中是否存在该键。如果映射中键不存在，将触发断点

```
FString Val7 = FruitMap[4]; // Val4 == "Kiwi"
FString Val8 = FruitMap[8]; // Assert!
```

### Find

按键查找

- 如果映射包含该键，Find 将返回指向元素数值的指针；如果映射不包含该键，则返回null。
- 在常量映射上调用 Find，返回的指针也将为常量。

```c++
FString* Ptr4 = FruitMap.Find(4); // *Ptr7 == "Kiwi"
FString* Ptr8 = FruitMap.Find(8); //  Ptr8 == nullptr
```

### FindOrAdd

将返回对与给定键关联的值的引用。

- 如果映射中不存在该键，FindOrAdd 将返回新创建的元素（使用给定键和默认构建值），该元素也会被添加到映射。
- FindOrAdd 可修改映射，因此仅适用于非常量映射。

```
FString& Ref4 = FruitMap.FindOrAdd(4);//{ Key:4, Value:"Kiwi" },
FString& Ref7 = FruitMap.FindOrAdd(7);//{ Key:7, Value:"" },
```

### FindKey

执行逆向查找

- 返回指向与所提供值配对的第一个键的指针。搜索映射中不存在的值将返回空键。
- 按值查找比按键查找慢（线性时间）。这是因为映射按键排序，而非按值排序。
- 如果映射有多个具有相同值的键，FindKey 可返回其中任一键。

```
const int32* KeyMangoPtr   = FruitMap.FindKey(TEXT("Mango"));   // 	KeyMangoPtr   == 5
const int32* KeyKumquatPtr = FruitMap.FindKey(TEXT("Kumquat")); //  KeyKumquatPtr == nullptr
```

### GenerateKeyArray` 和 `GenerateValueArray

分别使用所有键和值的副本来填充 TArray。

在填充前清空所传递的数组，因此产生的元素数量始终等于映射中的元素数量

```c++
TArray<int32>   FruitKeys;
TArray<FString> FruitValues;
FruitKeys.Add(999);
FruitKeys.Add(123);
FruitMap.GenerateKeyArray  (FruitKeys);
FruitMap.GenerateValueArray(FruitValues);
// FruitKeys   == [ 5,2,7,4,3,9,8 ]
// FruitValues == [ "Mango","Pear","Pineapple","Kiwi","Orange","Melon","" ]
```

## 移除

### Remove

提供要移除元素的键。

- 返回值是被移除元素的数量。
- 如果映射不包含与键匹配的元素，则返回值可为零。
- 移除元素将在数据结构（在Visual Studio的观察窗口中可视化映射时可看到）中留下空位

```
FruitMap.Remove(8);
// FruitMap == [
//  { Key:5, Value:"Mango"     },
//  { Key:2, Value:"Pear"      },
//  { Key:7, Value:"Pineapple" },
//  { Key:4, Value:"Kiwi"      },
//  { Key:3, Value:"Orange"    },
//  { Key:9, Value:"Melon"     }
// ]
```

## FindAndRemoveChecked

可用于从映射移除元素并返回其值。

- 名称中的checked部分意味着将检查键是否存在，如果不存在，则出现Assert

~~~cpp
FString Removed7 = FruitMap.FindAndRemoveChecked(7);
// Removed7 == "Pineapple"
// FruitMap == [
//  { Key:5, Value:"Mango"  },
//  { Key:2, Value:"Pear"   },
//  { Key:4, Value:"Kiwi"   },
//  { Key:3, Value:"Orange" },
//  { Key:9, Value:"Melon"  }
// ]

FString Removed8 = FruitMap.FindAndRemoveChecked(8);
// Assert!
~~~



### RemoveAndCopyValue

移除元素，并将已移除元素的值复制到引用参数。如果映射中不存在指定的键，则输出参数将保持不变，函数将返回 false。

~~~cpp
FString Removed;
bool bFound2 = FruitMap.RemoveAndCopyValue(2, Removed);
// bFound2  == true
// Removed  == "Pear"
// FruitMap == [
//  { Key:5, Value:"Mango"  },
//  { Key:4, Value:"Kiwi"   },
//  { Key:3, Value:"Orange" },
//  { Key:9, Value:"Melon"  } ]

bool bFound8 = FruitMap.RemoveAndCopyValue(8, Removed);
// bFound8  == false
// Removed  == "Pear", i.e. unchanged
// FruitMap == [
//  { Key:5, Value:"Mango"  },
//  { Key:4, Value:"Kiwi"   },
//  { Key:3, Value:"Orange" },
//  { Key:9, Value:"Melon"  } ]
~~~

### Empty&Reset

将映射中的所有元素移除。

- Empty 可采用参数指示映射中保留的slack量
- Reset 则是尽可能多地留出slack量。

~~~cpp
TMap<int32, FString> FruitMapCopy = FruitMap;
// FruitMapCopy == [
//  { Key:5, Value:"Mango"  },
//  { Key:4, Value:"Kiwi"   },
//  { Key:3, Value:"Orange" },
//  { Key:9, Value:"Melon"  }
// ]

FruitMapCopy.Empty();       // We could also have called Reset() here.
// FruitMapCopy == []
~~~

## 排序

TMap 可以进行排序。排序后，迭代映射会以排序的顺序显示元素，但下次修改映射时，排序可能会发生变化。排序是不稳定的，因此等值元素在MultiMap中可能以任何顺序出现。

### KeySort&ValueSort

按键和值进行排序。两个函数均使用二元谓词来进行排序

~~~cpp
FruitMap.KeySort([](int32 A, int32 B) {
    return A > B; // sort keys in reverse
});
// FruitMap == [
//  { Key:9, Value:"Melon"  },
//  { Key:5, Value:"Mango"  },
//  { Key:4, Value:"Kiwi"   },
//  { Key:3, Value:"Orange" }
// ]

FruitMap.ValueSort([](const FString& A, const FString& B) {
    return A.Len() < B.Len(); // sort strings by length
});
// FruitMap == [
//  { Key:4, Value:"Kiwi"   },
//  { Key:5, Value:"Mango"  },
//  { Key:9, Value:"Melon"  },
//  { Key:3, Value:"Orange" }
// ]
~~~

## 运算符

TMap 是常规值类型，可通过标准复制构造函数或赋值运算符进行复制。因为映射严格拥有其元素，复制映射的操作是深层的，所以新的映射将拥有其自己的元素副本。

```cpp
TMap<int32, FString> NewMap = FruitMap;
NewMap[5] = "Apple";
NewMap.Remove(3);
// FruitMap == [
//  { Key:4, Value:"Kiwi"   },
//  { Key:5, Value:"Mango"  },
//  { Key:9, Value:"Melon"  },
//  { Key:3, Value:"Orange" }
// ]
// NewMap == [
//  { Key:4, Value:"Kiwi"  },
//  { Key:5, Value:"Apple" },
//  { Key:9, Value:"Melon" }
// ]
```

### MoveTemp

可调用移动语义。在移动后，源映射必定为空

~~~cpp
FruitMap = MoveTemp(NewMap);
// FruitMap == [
//  { Key:4, Value:"Kiwi"  },
//  { Key:5, Value:"Apple" },
//  { Key:9, Value:"Melon" }
// ]
// NewMap == []
~~~

## Slack

- Slack是不包含元素的已分配内存。调用 Reserve 可分配内存，无需添加元素；
- 通过非零slack参数调用 Reset 或 Empty 可移除元素，无需将其使用的内存取消分配。
- Slack优化了将新元素添加到映射的过程，因为可以使用预先分配的内存，而不必分配新内存。
- 它在移除元素时也十分实用，因为系统不需要将内存取消分配。在清空希望用相同或更少的元素立即重新填充的映射时，此方法尤其有效。
- TMap 不像 TArray 中的 Max 函数那样可以检查预分配元素的数量。

### Reset

~~~cpp
FruitMap.Reset();
// FruitMap == [<invalid>, <invalid>, <invalid>]
// 内存不释放
~~~

### Reserve

预先分配映射

~~~cpp
FruitMap.Reserve(10);
for (int32 i = 0; i < 10; ++i)
{
    FruitMap.Add(i, FString::Printf(TEXT("Fruit%d"), i));
}
// FruitMap == [
//  { Key:9, Value:"Fruit9" },
//  { Key:8, Value:"Fruit8" },
//  ...
//  { Key:1, Value:"Fruit1" },
//  { Key:0, Value:"Fruit0" }
// ]
~~~



### Collapse&Shrink

可移除 TMap 中的全部slack。

- Shrink 将从容器的末端移除所有slack，遇到有效元素就停止，不会清除中间的Slack

~~~cpp
for (int32 i = 0; i < 10; i += 2)
{
    FruitMap.Remove(i);
}
// FruitMap == [
//  { Key:9, Value:"Fruit9" },
//  <invalid>,
//  { Key:7, Value:"Fruit7" },
//  <invalid>,
//  { Key:5, Value:"Fruit5" },
//  <invalid>,
//  { Key:3, Value:"Fruit3" },
//  <invalid>,
//  { Key:1, Value:"Fruit1" },
//  <invalid>
// ]
FruitMap.Shrink();
// FruitMap == [
//  { Key:9, Value:"Fruit9" },
//  <invalid>,
//  { Key:7, Value:"Fruit7" },
//  <invalid>,
//  { Key:5, Value:"Fruit5" },
//  <invalid>,
//  { Key:3, Value:"Fruit3" },
//  <invalid>,
//  { Key:1, Value:"Fruit1" }
// ]
//代码中，Shrink 只删除了一个无效元素，因为末端只有一个空元素
~~~

### Compact

将空白空间组合在一起，为调用 Shrink 做好准备

~~~cpp
FruitMap.Compact();
// FruitMap == [
//  { Key:9, Value:"Fruit9" },
//  { Key:7, Value:"Fruit7" },
//  { Key:5, Value:"Fruit5" },
//  { Key:3, Value:"Fruit3" },
//  { Key:1, Value:"Fruit1" },
//  <invalid>,
//  <invalid>,
//  <invalid>,
//  <invalid>
// ]
FruitMap.Shrink();
// FruitMap == [
//  { Key:9, Value:"Fruit9" },
//  { Key:7, Value:"Fruit7" },
//  { Key:5, Value:"Fruit5" },
//  { Key:3, Value:"Fruit3" },
//  { Key:1, Value:"Fruit1" }
// ]
~~~



