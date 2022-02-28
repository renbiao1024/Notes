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

# 类型

## UObject

- UObject提供的元数据、反射生成、GC垃圾回收、序列化、编辑器可见，Class Default Object等

![img](InsideUE4.assets/v2-750c05a282e8784c3af5815a481d549e_720w.png)

## Actor

- 派生自UObject
- 本身不带`transform`,可以用`getactorlocation`获取位置

~~~cpp
template<class T>
static FORCEINLINE FVector GetActorLocation(const T* RootComponent)
{	//实质上获取的是跟组件的位置
    return (RootComponent != nullptr) ? RootComponent->GetComponentLocation() : FVector(0.f,0.f,0.f);
}
bool AActor::SetActorLocation(const FVector& NewLocation, bool bSweep, FHitResult* OutSweepHitResult, ETeleportType Teleport)
{
    if (RootComponent)
    {
        const FVector Delta = NewLocation - GetActorLocation();
        return RootComponent->MoveComponent(Delta, GetActorQuat(), bSweep, OutSweepHitResult, MOVECOMP_NoFlags, Teleport);
    }
    else if (OutSweepHitResult)//检测新位置有碰撞
    {
        *OutSweepHitResult = FHitResult();
    }
    return false;
}
~~~

- Replication（网络复制）,Spawn（生生死死），Tick(有了心跳)

- 一些不在世界里展示的“不可见对象”也可以是Actor，如`AInfo`(派生类`AWorldSetting,AGameMode,AGameSession,APlayerState,AGameState`等)，`AHUD,APlayerCameraManager`等，代表了这个世界的某种信息、状态、规则。

- 能接收处理Input事件的能力，其实也是转发到内部的UInputComponent*

![img](InsideUE4.assets/v2-9348f6bdadbe382a505aff9be7d5d99e_720w.png)

## Component

- 在`actor`内的`TSet<UActorComponent*> OwnedComponents` 保存着这个`Actor`所拥有的所有`Component`,一般其中会有一个`SceneComponent`作为`RootComponent`。
- SceneComponent提供了两大能力：一是Transform，二是SceneComponent的互相嵌套。

> 为什么不允许所有component互相嵌套？如Component之间如何互相依赖，如何互相通信，嵌套过深导致的接口便利损失和性能损耗，真正一个让你随便嵌套的组件模式可能会在使用上更容易出问题

- UE里是通过Child:AttachToActor或Child:AttachToComponent来创建父子连接的。其实是不同Actor的SceneComponent之间有父子关系，而Actor本身其实并不太关心。

~~~cpp
void AActor::AttachToActor(AActor* ParentActor, const FAttachmentTransformRules& AttachmentRules, FName SocketName)
{
    if (RootComponent && ParentActor)
    {
        USceneComponent* ParentDefaultAttachComponent = ParentActor->GetDefaultAttachComponent();
        if (ParentDefaultAttachComponent)
        {
            //将自己的跟组件附加到父类actor的socket上，把当前Actor当作对方哪个SceneComponent的子
            RootComponent->AttachToComponent(ParentDefaultAttachComponent, AttachmentRules, SocketName);
        }
    }
}
void AActor::AttachToComponent(USceneComponent* Parent, const FAttachmentTransformRules& AttachmentRules, FName SocketName)
{
    if (RootComponent && Parent)
    {
        RootComponent->AttachToComponent(Parent, AttachmentRules, SocketName);
    }
}
~~~

~~~cpp
/** 附加组件的规则 - 需要与 EDetachmentRule 保持同步 */
UENUM()
enum class EAttachmentRule : uint8
{
    /** 将当前相对转换保留为对新父项的相对转换。 */
    KeepRelative,
    /** 自动计算相对变换，以便附加的组件保持相同的世界变换。 */
    KeepWorld,
    /** 对齐变换到连接点*/
    SnapToTarget,
};
~~~

~~~cpp
void UChildActorComponent::OnRegister()//注册时
{
    Super::OnRegister();
    if (ChildActor)
    {
        if (ChildActor->GetClass() != ChildActorClass)
        {
            DestroyChildActor();
            CreateChildActor();
        }
        else//已经存在直接添加上去
        {
            ChildActorName = ChildActor->GetFName();
            USceneComponent* ChildRoot = ChildActor->GetRootComponent();
            if (ChildRoot && ChildRoot->GetAttachParent() != this)
            {
				/*将新执行组件附加到此组件
                我们无法在CreateChildActor中附加，因为它设置了中间移动性
                导致移动设置不一致
                所以移动附加到注册中发生*/
                ChildRoot->AttachToComponent(this, FAttachmentTransformRules::SnapToTargetNotIncludingScale);
            }
            //网络复制
            SetIsReplicated(ChildActor->GetIsReplicated());
        }
    }
    else if (ChildActorClass)/
    {
        CreateChildActor();
    }
}
void UChildActorComponent::OnComponentCreated()
{
    Super::OnComponentCreated();
    CreateChildActor();
}
~~~



![img](InsideUE4.assets/v2-825217f7dc7b7f3ce2068433b037dfb7_720w.png)

## Level

- 关卡蓝图本身就是一个看不见的Actor，可以添加组件

~~~cpp
void ALevelScriptActor::PreInitializeComponents()
{
    if (UInputDelegateBinding::SupportsInputDelegate(GetClass()))
    {
        // create an InputComponent object so that the level script actor can bind key events
        InputComponent = NewObject<UInputComponent>(this);
        InputComponent->RegisterComponent();
        UInputDelegateBinding::BindInputDelegates(GetClass(), InputComponent);
    }
    Super::PreInitializeComponents();
}
~~~



![img](InsideUE4.assets/v2-bca44e1f846c37b12f08bc0a6659b4ae_720w.png)

## World

- 可以用SubLevel的方式把Level拼装起来

- 也可以WorldComposition的方式自动把项目里的所有Level都组合起来，并设置摆放位置

- 一个World里有多个Level，这些Level在什么位置，是在一开始就加载进来，还是Streaming运行时加载。UE里每个World支持一个PersistentLevel和多个其他Level

> Persistent的意思是一开始就加载进World，Streaming是后续动态加载的意思。Levels里保存有所有的当前已经加载的Level，StreamingLevels保存整个World的Levels配置列表。PersistentLevel和CurrentLevel只是个快速引用。

![img](InsideUE4.assets/v2-41963e6f39bcefb2d799d31bec703759_720w.png)

- 世界设置

~~~cpp
AWorldSettings* UWorld::GetWorldSettings( bool bCheckStreamingPesistent, bool bChecked ) const
{
    checkSlow(IsInGameThread());
    AWorldSettings* WorldSettings = nullptr;
    if (PersistentLevel)//游戏第一个场景（主场景）为主设置其他为辅的Level配置系统
    {
        WorldSettings = PersistentLevel->GetWorldSettings(bChecked);
        if( bCheckStreamingPesistent )
        {
            if( StreamingLevels.Num() > 0 &&
                StreamingLevels[0] &&
                StreamingLevels[0]->IsA<ULevelStreamingPersistent>()) 
            {
                ULevel* Level = StreamingLevels[0]->GetLoadedLevel();
                if (Level != nullptr)
                {
                    WorldSettings = Level->GetWorldSettings();
                }
            }
        }
    }
    return WorldSettings;
}
~~~

- Levels共享着World的一个PhysicsScene，这也意味着Levels里的Actors的物理实体其实都是在World里的

- Level作为Actor的容器，同时也划分了World，一方面支持了Level的动态加载，另一方面也允许了团队的实时协作，大家可以同时并行编辑不同的Level。