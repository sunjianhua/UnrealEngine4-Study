# Unreal Engine 4 Scripting with C++ Cookbook

## Creating Classes

### Making a UCLASS – deriving from UObject

```
打开类创建窗口：编辑器模式，File菜单的子菜单New C++ Class
C++类转换为蓝图：C++类必须有UCLASS(Blueprintable)，否则在编辑器里Create Blueprint class based On XXX 按钮是灰的，如果父类已有UCLASS(Blueprintable)，子类只有UCLASS()也可以

UCLASS( Blueprintable, BlueprintType )
BlueprintType：好像解释为，可以在其它蓝图里创建这个类型变量，但是实验下，没有这个也可以，许多存疑？？？

C++类转换蓝图，可以理解为：实现了一个继承c++的蓝图类
蓝图类也是类概念，拖拽蓝图类到场景，就创建了一个实例 / 对象
```

### Creating a user-editable UPROPERTY

```
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Stats)
以下描述是在编辑器模式的情形

下面的蓝图实例是指编辑模式下的蓝图实例而不是运行后的蓝图实例

EditAnywhere：属性字段能在蓝图类或蓝图实例里编辑
EditDefaultsOnly：属性字段在蓝图类可以编辑，在蓝图实例里不能编辑
EditInstanceOnly：属性字段在蓝图实例可以编辑，在蓝图类不可以编辑
BlueprintReadWrite：属性字段可以读写
BlueprintReadOnly: 属性字段可以读【就是灰色显示】
Category：属性字段分组
```

### Accessing a UPROPERTY from Blueprints
```
这个实验失败
```

### Specifying a UCLASS as the type of a UPROPERTY
UClass 实例？？？？？？
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Unit)
TSubclassOf<UObject> UClassOfPlayer;

这个没有搞明白？？？
UPROPERTY(EditAnywhere, meta = (MetaClass = "GameMode"), Category = Unit)
FStringClassReference UClassGameMode;

### Creating a Blueprint from your custom UCLASS
这个主要是讲怎样在编辑器模式下，通过Window | Developer Tools | Class Viewer，打开Class的树列表，然后在自定义类上创建蓝图的操作流程

### Instantiating UObject-derived classes
NewObject【4.8版本后ConstructObject不建议使用】

下面这个写法不用通过TSubclassOf<UObject>到蓝图里指定UClass instance
NewObject<UObject>(GetTransientPackage(), UObject::StaticClass());

### Destroying UObject-derived classes
ConditionalBeginDestroy，给要销毁的UObject打标记，根据下面配置的时间，到了后没有用就回收
x:\UE_4.15\Engine\Config\BaseEngine.ini 
gc.TimeBetweenPurgingPendingKillObjects=60

如果要立即执行回收
GetWorld()->ForceGarbageCollection(true)

组件销毁
USceneComponent::DestroyComponent

### Creating a USTRUCT
USTRUCT标记的struct的.h文件，需要手动加上【#include "??????.generated.h"】在编译时会生成实际的【??????.generated.h】文件
UCLASS标记的class的.h文件，好像不需要手动加上，编译时会自己加？？？

USTRUCT标记的struct需要GENERATED_USTRUCT_BODY()，这个也需要手动加上

### Creating a UENUM()
UENUM标记enum可以在编辑器里用，enum里的值用UMETA标记值在编辑器里显示的文字

要在编辑器用，需要用TEnumAsByte声明enum：TEnumAsByte<enumType> enumVar 

### Creating a UFUNCTION
函数加上下面的就能在蓝图使用了
UFUNCTION(BlueprintCallable, Category = Properties)

## Memory Management and Smart Pointers
### Unmanaged memory – using malloc()/free()
略
### Unmanaged memory – using new/delete
略
### Managed memory – using NewObject< > and ConstructObject< >
略
### Managed memory – deallocating memory
略
### Managed memory – smart pointers (TSharedPtr, TWeakPtr, TAutoPtr) to trackan object
MakeShareable
### Using TScopedPointer to track an object
### Unreal's garbage collection system and UPROPERTY()
### Forcing garbage collection
### Breakpoints and stepping through code
### Finding bugs and using call stacks
### Using the Profiler to identify hot spots

## Actors and Components
### Creating a custom Actor in C++
### Instantiating an Actor using SpawnActor
### Destroying an Actor using Destroy and a Timer
### Destroying an Actor after a delay using SetLifeSpan
### Implementing the Actor functionality by composition
### Loading assets into components using FObjectFinder
### Implementing the Actor functionality by inheritance
### Attaching components to create a hierarchy
### Creating a custom Actor Component
### Creating a custom Scene Component
### Creating a custom Primitive Component
### Creating an InventoryComponent for an RPG
### Creating an OrbitingMovement Component
### Creating a building that spawns units

## Handling Events and Delegates
### Handling events implemented via virtual functions
### Creating a delegate that is bound to a UFUNCTION
### Unregistering a delegate
### Creating a delegate that takes input parameters
### Passing payload data with a delegate binding
### Creating a multicast delegate
### Creating a custom Event
### Creating a Time of Day handler
### Creating a respawning pickup for an First Person Shooter
