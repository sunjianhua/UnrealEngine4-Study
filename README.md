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

### 路径信息 (2016-10-13 ~ 2016-10-13)
1. FPaths::GameDir()

### Engine\Source\Runtime\Core\Public\Misc (2016-10-14 ~ 2016-10-14)
1. 想到有什么基本功能要写的，就先去这个目录下查一下，是否已经实现

### 定时器 (2016-10-17 ~ 2016-10-17)
GetWorldTimerManager().SetTimer(??????)

### 编译代码 (2016-10-18 ~ 2016-10-18)
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
UGameplayStatics::GetAllActorsOfClass
for (FActorIterator It(GetWorld()); It; ++It)

## 代码到蓝图

## ModuleRules (2016-10-14 ~ )
