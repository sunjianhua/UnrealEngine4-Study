## UMG
[使用UMG的用户接口](https://docs.unrealengine.com/latest/CHN/Programming/Tutorials/UMG/index.html)
1. 在 .Build.CS 文件，添加"UMG"、"Slate"模块
    ~~~
    PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "UMG" });
    PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });
    ~~~
2.  在窗口显示一个Widget
    ~~~
    NewWidget = CreateWidget<UUserWidget>(GetWorld(), NewWidgetClass);
    NewWidget->AddToViewport();
    ~~~
3. 在编辑器里新建蓝图界面
    ~~~
    1. Content Browser（内容浏览器） 中的"Add New"（添加新控件）
    2. User Interface（用户接口）分类下找到 Widget Blueprint（控件蓝图）类
    ~~~

## Slate
[Slate,_Hello](https://wiki.unrealengine.com/Slate,_Hello)

1. 在 .Build.CS 文件，添加"UMG"、"Slate"模块
    ~~~
    PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });
    ~~~
2. 在窗口显示一个Widget
    ~~~
    NewWidget = SNew(SCompoundWidget);
    GEngine->GameViewport->AddViewportWidgetContent(SNew(SWeakWidget).PossiblyNullContent(NewWidget.ToSharedRef()));
    NewWidget->SetVisibility(EVisibility::Visible);
    ~~~
### 编写界面

#### 基本
        1. 继承SCompoundWidget
        2. void Construct(const FArguments& InArgs);

#### 声明式语法
        ~~~
        SLATE_BEGIN_ARGS
        SLATE_ARGUMENT
        SLATE_EVENT
        SLATE_NAMED_SLOT
        SLATE_ATTRIBUTE
        SLATE_END_ARGS
        ~~~

#### 构成【主要是理解Slot】
    ~~~
    SNew(SHorizontalBox)
    +SHorizontalBox::Slot()
    [
        SNew(SButton)
        .Style(TEXT("ToolBarButton"))
        .OnClicked( InKismet2.ToSharedRef(), &FKismet::Compile_OnClicked )
        .IsEnabled( InKismet2.ToSharedRef(), &FKismet::InEditingMode )
        .Content()
        [
            SNew(SVerticalBox)
            +SVerticalBox::Slot()
            .Padding( 1.0f )
            .HAlign(HAlign_Center)
            [
                SNew(SImage)
                .Image(this, &SBlueprintEditorToolbar::GetStatusImage)
                .ToolTipText(this, &SBlueprintEditorToolbar::GetStatusTooltip)
            ]
            +SVerticalBox::Slot()
            .Padding( 1.0f )
            .HAlign(HAlign_Center)
            [
                SNew(STextBlock)
                .Text(LOCTEXT("CompileButton", "Compile"))
                .Font( FEditorStyle::GetFontStyle( FName( "ToolBarButton.LabelFont" ) ) )
                .ToolTipText(LOCTEXT("CompileButton_Tooltip", "Recompile the blueprint"))
            ]
        ]
    ]
    ~~~
    