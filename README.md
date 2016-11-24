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

## 代码到蓝图

## ModuleRules (2016-10-14 ~ )
