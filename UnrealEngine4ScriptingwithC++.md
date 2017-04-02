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
这个实现的真牛，模板化指针类型的成员变量，优点是在编辑里把符合模板类型的都列出来
UPROPERTY(EditAnywhere, BlueprintReadWrite, Category = Unit)
TSubclassOf<UObject> UClassOfPlayer;

这个没有搞明白？？？
UPROPERTY(EditAnywhere, meta = (MetaClass = "GameMode"), Category = Unit)
FStringClassReference UClassGameMode;

### Creating a Blueprint from your custom UCLASS
这个主要是讲怎样在编辑器模式下，通过Window | Developer Tools | Class Viewer，打开Class的树列表，然后在自定义类上创建蓝图的操作流程

### Instantiating UObject-derived classes
创建对象的两个方法
ConstructObject
NewObject

### Destroying UObject-derived classes
ConditionalBeginDestroy
GetWorld()->ForceGarbageCollection(true)

组件销毁
USceneComponent::DestroyComponent

BaseEngine.ini 
gc.TimeBetweenPurgingPendingKillObjects=60

MakeShareable
### Creating a USTRUCT

### Creating a UENUM()

### Creating a UFUNCTION
