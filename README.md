# Unreal Engine4 学习

项目以目录划分，只上传新增代码，不包含Unreal Engine4创建项目自动生成文件

常用的：UGameplayStatics

## 基本概念

### 命令行参数 (2016-10-12 ~ 2016-10-12)
1. 获得命令行：FCommandLine::Get()
2. 解析命令行：FParse::Value

### 配置文件 (2016-10-13 ~ 2016-10-13)
1. 声明配置文件：FConfigFile
2. 打开配置文件：FConfigFile::Read
3. 获得分组内容：FConfigFile::Find
4. 配置文件分组：FConfigSection
5. 获得分组子项: FConfigSection::Find

### Engine\Source\Runtime\Core\Public\Misc
1. 想到有什么基本功能要写的，就先去这个目录下查一下，是否已经实现

### 定时器
GetWorldTimerManager().SetTimer

### 编译代码
UnrealEngine 4 下载源码，需要在UnrealEngine账户把自己的github账号设置上，不然在github看不到项目

## Additional Non-asset Directories to Package 取资源
FString Contents;
FString Filename = FPaths::GameContentDir() + TEXT("Resource/conf/role/Role.conf");
if (FFileHelper::LoadFileToString(Contents, *Filename))
{
	// do something with Contents
}

## 线程锁定修改单值
FPlatformAtomics::InterlockedExchange(变量指针, 新值);

##互斥锁
FScopeLock ScopeLock(&Lock);

## 格式化字符串
FString::Printf(TEXT("%s 你好。"), TEXT("朋友"))

## 输出日志
UE_LOG(LogTemp, Warning, TEXT("%s 你好。"), TEXT("朋友"));

## 输出信息到屏幕
GEngine->AddOnScreenDebugMessage(-1, 5.0f, FColor::Yellow, TEXT("朋友 你好。"));

## 输出字符串到屏幕
DrawDebugString
UKismetSystemLibrary::DrawDebugString

## 在代码显示/隐藏鼠标的一个方法
FSlateApplication::Get().GetPlatformApplication()->Cursor->Show(false);

## 鼠标检测
UWorld::LineTraceSingleByChannel
FMath::LinePlaneIntersection
APlayerController::GetHitResultAtScreenPosition
APlayerController::GetHitResultUnderCursor

## 剪贴板内容拷贝
FPlatformMisc::ClipboardCopy

## 设置AActor旋转
1. USceneComponent* SceneComponent = AActor::GetRootComponent();
2. SceneComponent->SetRelativeRotation(???);
3. SceneComponent->RelativeRotation = ???;
4. AActor::ReregisterAllComponents();

## 获得当前场景所有AActor
1. UGameplayStatics::GetAllActorsOfClass
2. for (FActorIterator It(GetWorld()); It; ++It)

## 长短文件名 UE4里对短文件名<不包含路径信息>会进行目录遍历，比较耗时，长文件名<包含路径信息>基本是定位查找，比较快
FPackageName::IsShortPackageName

UGameplayStatics::OpenLevel 会根据长短文件名做不同文件查找操作

## Converts a long package name to a file name with the supplied extension
FPackageName::LongPackageNameToFilename

### 路径信息
1. FPaths::GameDir()
2. FPaths::GameSavedDir()
3. ......

## 判断文件是否存在
1. FPlatformFileManager::Get().GetPlatformFile().FileExists()
2. IFileManager::Get().FileExists

## 判断目录是否存在
1. IFileManager::Get().DirectoryExists

## 获取目录下所有文件或子目录名
IFileManager::Get().FindFiles

## 删除文件
IFileManager::Get().Delete

## 四种加载资源方式
### 1. 如果该蓝图有C++类(或者说是从C++类创建的蓝图),直接进行加载
ATemp* spawnActor = GetWorld()->SpawnActor<ATemp>(ATemp::StaticClass());
### 2. 通过ConstructorHelpers加载
static ConstructorHelpers::FClassFinder<AActor> bpClass(TEXT("/Game/BluePrint/TestObj"));  
if(bpClass.Class != NULL)
{
    GetWorld()->SpawnActor(bpClass.Class);
}
### 3. 通过FStringAssetReference加载
FStringAssetReference asset = "Blueprint'/Game/BluePrint/TestObj.TestObj'";  
    UObject* itemObj = asset.ResolveObject();  
    UBlueprint* gen = Cast<UBlueprint>(itemObj);  
    if (gen != NULL)   
    {
        AActor* spawnActor = GetWorld()->SpawnActor<AActor>(gen->GeneratedClass);  
    }
### 4. 通过StaticLoadObject加载
UObject* loadObj = StaticLoadObject(UBlueprint::StaticClass(), NULL, TEXT("Blueprint'/Game/BluePrint/TestObj.TestObj'"));  
if (loadObj != nullptr)
{
    UBlueprint* ubp = Cast<UBlueprint>(loadObj);  
    AActor* spawnActor = GetWorld()->SpawnActor<AActor>(ubp->GeneratedClass);
    UE_LOG(LogClass, Log, TEXT("Success"));
}

## 获得UE4 版本
GEngineVersion

## 定时器回调
FTimerHandle TimerHandle;
FTimerDelegate TimerDelegate;
TimerDelegate.BindLambda([this, HitMeshComponent] {});

GetWorld()->GetTimerManager().SetTimer(TimerHandle, TimerDelegate, 0.5, false);

## 显示或关闭组件边框线
UPrimitiveComponent::SetRenderCustomDepth

## 时间线
蓝图组件：TimeLine

## 代码到蓝图

## ModuleRules (2016-10-14 ~ )


https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/CreatingLayeredMaterials/index.html
WorldAlignedBlend
WorldAlignedNormal
WorldAlignedTexture
SlopeMask
MatlayerBlend_Simple
MatlayerBlend_Standard
ComponentMask
MakeMaterialAttributes
MatLayerBlend_StandardWithDisplacement
【
WorldAlignedTexture：全局一致纹理【函数用于在全局空间中的对象表面上平铺纹理，此平铺与该对象的大小或旋转无关。此函数允许您指定投射纹理的方向，并按全局单位（而非纹理大小的百分比）进行比例调整】
因为此函数在全局空间内平铺纹理，所以需要注意，任何以此方式处理纹理的动画对象都会发生纹理“漂浮”，即纹理保持原位置不动，而对象在其下方滑动
】
【
HeightLerp：高度插值【函数允许您根据高度贴图和过渡阶段值，在 2 个纹理之间执行线性插值。这允许您沿着发生插值的高度贴图来调整值】
】
PixelDepth：像素深度【表达式输出当前所渲染像素的深度，即从摄像机开始计算的距离】
SceneDepth：场景深度【类似于 PixelDepth（像素深度），PixelDepth（像素深度）只能在当前所绘制像素处进行深度取样，而 SceneDepth（场景深度）可以在 任何位置 进行深度取样】
Divide
LinearInterpolate
FuzzyShading1
Clamp
Landscape Layer Sample
Bump Offset: 设置凹凸贴图偏移【https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/BumpOffset/index.html】
Panner：产生 UV 坐标动画【https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/AnimatingUVCoords/index.html】
后期处理体积【Post Process Volume】
Detail Texturing：细节纹理化【https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/DetailTexturing/index.html】

https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/ExpressionReference/Vector/index.html#cameravectorws
https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/Making_Functions/index.html
{
	ActorPositionWS：Actor 全局空间位置【输出 Vector3 (RGB) 数据，该数据代表使用此材质的对象在全局空间中的位置】
	CameraWorldPosition：摄像机全局空间位置【表达式输出三通道矢量值，该值代表摄像机在全局空间中的位置】
	CameraVectorWS：摄像机全局空间矢量【表达式输出一个三通道矢量值，该值代表摄像机相对于表面的方向，即从像素到摄像机的方向】
}

https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/Fresnel/index.html

坐标表达式
https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/ExpressionReference/Coordinates/index.html

LandscapeLayerWeight：地形图层权重节点【表现允许材质网络进行混合。混合的基础是从材质应用的地形上所获取的相关图层权重】
LandscapeLayerSample：这个会产生绘制层
LandscapeLayerBlend：【LB Height Blend，这个选项能产生沙子层里的沙子落在岩石层里的岩石缝隙里？？？？？？】
