Unreal Engine 4 里的 DELEGATE 就是 C++ 函数指针的封装？？？ 因为没有跟相关代码，以下的总结纯粹的是自以为是的总结，可能完全的错误

## 函数指针作用
1. C / C++ 函数指针的主要作用就是 作为其它函数的参数或返回值使用
2. 基于此，最主要作用就是方便实现回调逻辑

## 函数指针的三步实现
 1. 声明函数指针：就是函数名前加个 * 
 2. 定义函数指针变量，把符合函数指针格式【返回值和参数类型、参数个数相同】的函数的名字赋予函数指针变量
 3. 函数指针变量加个 () 就可以调指向的函数了

## 函数指针封装
 1. 因为函数指针指向的函数，只要求返回值、参数类型、参数个数相同，所以就可以用模板或宏把参数和返回值相同的归类下，方便使用
 2. 在 Unreal Engine 4 里基于此的封装就是

    | Function signature        | Declaration macro |
    | -------------             | -------------     |
    | void Function()									|	DECLARE_DELEGATE( DelegateName ) |
	| void Function( <Param1> )							|	DECLARE_DELEGATE_OneParam( DelegateName, Param1Type ) |
	| void Function( <Param1>, <Param2> )				|	DECLARE_DELEGATE_TwoParams( DelegateName, Param1Type, Param2Type ) |
	| void Function( <Param1>, <Param2>, ... )			|	DECLARE_DELEGATE_<Num>Params( DelegateName, Param1Type, Param2Type, ... ) |
	| <RetVal> Function()								|	DECLARE_DELEGATE_RetVal( RetValType, DelegateName ) |
	| <RetVal> Function( <Param1> )						|	DECLARE_DELEGATE_RetVal_OneParam( RetValType, DelegateName, Param1Type ) |
	| <RetVal> Function( <Param1>, <Param2> )			|	DECLARE_DELEGATE_RetVal_TwoParams( RetValType, DelegateName, Param1Type, Param2Type ) |
	| <RetVal> Function( <Param1>, <Param2>, ... )		|	DECLARE_DELEGATE_RetVal_<Num>Params( RetValType, DelegateName, Param1Type, Param2Type, ... ) |
 3. Unreal Engine 4 里封装了一次调用多个函数指针的功能：MULTICAST_DELEGATE【这就需要额外多做一步操作，把需要一次调用的函数指针保存在一起】
 4. Unreal Engine 4 里在 C++ 定义的函数指针要在蓝图里使用，可以用：DYNAMIC_DELEGATE
 5. 基于以上，在 Unreal Engine 4 里有三个不同的 delegate 类型

    | 类型                      | 用法               |
    | -------------             | -------------     |
    |Single-cast delegates									|	DECLARE_DELEGATE...() |
    |Multi-cast delegates									|	DECLARE_MULTICAST_DELEGATE...() |
    |Dynamic (UObject, serializable) delegates				|	DECLARE_DYNAMIC_DELEGATE...() |
 6. 有了封装好的函数指针声明，定义一个函数指针变量就比较方便了，比如：
    ~~~
    DECLARE_DELEGATE(FPrintSomeText)
    
    FPrintSomeText PrintSomeText;
    ~~~
 7. 给函数指针赋值，在 Unreal Engine 4 里用的是：BindSP、BindRaw、BindStatic、BindLambda 等，这些具体实现可以看 TBaseDelegate 【 DELEGATE 的绑定 】

    | 函数                      | 描述               |
    | -------------             | -------------     |
    | BindLambda                | 绑定 C++ lambda 函数    |
    | BindRaw					| 绑定到一个原始的C++指针全局函数代理上。原始指针不使用任何引用，所以如果从代理的底层删除了该对象，那么调用它可能是不安全的。因此，当调用Execute()时一定要小心! |
    | BindSP                    | 绑定一个基于共享指针的成员函数代理。共享指针代理保持到您的对象的弱引用。您可以使用 ExecuteIfBound() 来调用它们    |
    | BindUObject               | 绑定一个基于UObject的成员函数代理。UObject 代理保持到您的对象的弱引用。您可以使用 ExecuteIfBound() 来调用它们    |
    | UnBind                    | 解除绑定该代理     |

 8. 调用函数指针指向的函数，在 Unreal Engine 4 里用的是：ExecuteIfBound、ExecuteIfSafe、Execute 等？ 这些具体实现可以看 DelegateSignatureImpl.inl 【 DELEGATE 的执行 】

## 单播代理
[官方文档](https://docs.unrealengine.com/latest/CHN/Programming/UnrealArchitecture/Delegates/index.html)

## 多播代理
[官方文档](https://docs.unrealengine.com/latest/CHN/Programming/UnrealArchitecture/Delegates/Multicast/index.html)

## 动态代理
[官方文档](https://docs.unrealengine.com/latest/CHN/Programming/UnrealArchitecture/Delegates/Dynamic/index.html)

## 事件
【事件】是特定类型的多播代理，虽然任意类均可绑定事件，但只有声明事件的类可以调用事件 的 Broadcast、IsBound 和 Clear 函数。这意味着事件对象可在公共接口中公开，而无需让外部类访问这些敏感度函数。事件使用情况有：在纯抽象类中包含回调、限制外部类调用 Broadcast、IsBound 和 Clear 函数。

[官方文档](https://docs.unrealengine.com/latest/CHN/Programming/UnrealArchitecture/Delegates/Events/index.html)

## 实际应用
### 纯蓝图
1. 在 Blueprint Class 的 Add Custom Event

### C++ 实现 蓝图调用
