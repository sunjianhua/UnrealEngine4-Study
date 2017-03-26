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

### Specifying a UCLASS as the type of a UPROPERTY

### Instantiating UObject-derived classes

### Destroying UObject-derived classes

### Creating a USTRUCT

### Creating a UENUM()

### Creating a UFUNCTION
