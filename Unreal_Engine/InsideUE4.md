# 文件结构

打*表示可以gitignore

- ***Binaries**：生成的二进制文件
- **Config**：配置文件
- **Content**：资源，蓝图
- **DerivedDataCache（DDC）**：引擎针对平台特化后的资源版本
- ***Intermediate**：中间文件，临时文件

- ***Saved**：自动保存的文件，其他配置文件，日志文件，引擎崩溃日志，硬件信息，烘培信息数据等

- **Source**：代码文件

# 编译类型

- ![img](InsideUE4.assets/1E24B7132B5BFBD0C80359D93E3BD79A.png)

- ![img](InsideUE4.assets/E5BCEB5AB93B7993EF249E8AC7D750F4.png)

- ![img](InsideUE4.assets/D92DE227393661A2DDC9EB6C8554ABFF.png)

# 命名规定

- 模版类以`T`作为前缀，比如`TArray,TMap,TSet `
- `UObject`派生类都以`U`前缀 
- `AActor`派生类都以`A`前缀 
- `SWidget`派生类都以`S`前缀 
- 抽象接口以`I`前缀 
- 枚举以`E`开头 
- `bool`变量以`b`前缀，如`bPendingDestruction `
- 其他的大部分以`F`开头，如`FString,FName `
- `typedef`的以原型名前缀为准，如`typedef TArray FArrayOfMyTypes; `
- 在编辑器里和C#里，类型名是去掉前缀过的 
- `UHT (UnrealHeaderTool)`在工作的时候需要你提供正确的前缀，所以虽然说是约定，但你也得必须遵守。

# 编译系统

- UBT：ue4的自定义工具，来编译ue4的逐个模块并处理依赖。`Target.cs  Build.cs`都是为这个工具服务的
- UHT：ue4的c++代码解析生成工具，`UCLASS  include "xxx.generated.h"`都是为UHT提供信息来生成相应的C++反射代码