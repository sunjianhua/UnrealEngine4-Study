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
 7. 给函数指针赋值，在 Unreal Engine 4 里用的是：BindSP、BindRaw、BindStatic、BindLambda 等，这些具体实现可以看 TBaseDelegate
 8. 调用函数指针指向的函数，在 Unreal Engine 4 里用的是：ExecuteIfBound、Broadcast、ExecuteIfSafe、Execute 等？ 这些具体实现可以看 DelegateSignatureImpl.inl

## 实际应用
### 纯蓝图
1. 在 Blueprint Class 的 Add Custom Event

### C++ 实现 蓝图调用

