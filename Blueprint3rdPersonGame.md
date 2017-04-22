## [Blueprint 3rd Person Game]

[视频首页](https://docs.unrealengine.com/latest/INT/Videos/PLZlv_N0_O1ga0IoRrpI4xkX4qmCrhGu56/hRO82u1phyw/index.html)

### 01 - Introduction
略

### 02 - Project Creation & FBX Download
[下载资源](https://wiki.unrealengine.com/File:ThirdPerson_FBX.zip)

### 03 - FBX Importing & Using Skeletons
1. 导入模型：SK_Mannequin.FBX， Normal Import Method 选择 Import Normals
2. 导入动画

按住Alt，按住鼠标左键不，在编辑器里拖动，是在当前视点的旋转

### 04 - Intro to Persona
介绍编辑器里：骨骼/模型/动画 的操作界面

### 05 - Setting Up Inputs
在Project Settings的Engine -Input部分设置 Action/Axis Mappings

TODO：补图

### 06 - Basic Character Material
[材质 - 在材质中使用菲涅耳效果](https://docs.unrealengine.com/latest/CHN/Engine/Rendering/Materials/HowTo/Fresnel/index.html)

？？？

TODO：补图

### 07 - Blend Spaces
Blend Space：以时间线串联多个 Animation Sequence，形成一个复合的 Animation Sequence？？？

Blend Space：大概执行逻辑 ？？？

TODO：补图

### 08 - Intro to Animation Blueprints
创建一个Animation Blueprints

介绍编辑器中Animation Blueprints的操作界面和接口 与 普通Blueprints的区别

### 09 - Intro to State Machines
介绍状态机的机制

大概就是由：状态和状态间的转换条件组成

### 10 - Building the AnimGraph
在Animation Blueprints，创建一个状态机，包含运动和跳跃的状态转换

状态：播放动画

状态的转换条件：
1. 动画是否播放完，播放完跳转到下一个状态
2. 是否在空气中，确定是否跳还是落地

TODO：补图

### 11 - Animation BP EventGraph
在动画蓝图的Event Graph里把状态机条件转换时用到的几个状态变量给设置了，包括：速度和是否在空中

ACharacter 继承自 APawn，重写了虚函数 UPawnMovementComponent* GetMovementComponent()，返回的是 ACharacter 里定义的 UCharacterMovementComponent

UCharacterMovementComponent 继承自 UPawnMovementComponent

UPawnMovementComponent 继承自 UNavMovementComponent

UNavMovementComponent 继承自 UMovementComponent


UNavMovementComponent 有一个 virtual bool IsFalling() const 这个函数在 UNavMovementComponent并没真正实现，只是简单返回false

UCharacterMovementComponent 重写了 virtual bool IsFalling() const

UCharacterMovementComponent 有一个成员变量 TEnumAsByte<enum EMovementMode> MovementMode，记录当前移动状态

根据以上判断是否在空中：
1. 调用 IsFalling
2. 判断 MovementMode 是否为 MOVE_Falling

速度的设置，根据 UMovementComponent 的 Velocity ，取其长度作为速度值

### 12 - Character BP Components
1. 创建 Character 蓝图
2. 在 Character 蓝图 的 Mesh 组件，设置模型和动画蓝图
3. 增加 Spring Arm 组件，设置 Spring Arm 组件的 Use Pawn Control Rotation 为 选择
4. Spring Arm 组件，增加子组件 Camera
5. 点 Character BP 界面的 Class Settings 取消 Use Controller Rotation Roll 的选择
6. 选择 Character Movement 组件， 选择 Orient Rotation to Movement

### 13 - Character Keyboard & Mouse Controls
根据已经设置的按键事件，进行相应的按键事件处理，以下为在 Character BP 的 Event Graph 里的操作

1. 向前走：GetControlRotation 来确定 前后方向，然后执行 AddMovementInput
2. 左右走：GetControlRotation 来确定 左右方向，然后执行 AddMovementInput
3. 鼠标控制左右看：AddControllerYawInput
4. 鼠标控制上下看：AddControllerPitchInput
5. 跳：Jump

### 14 - Game Mode & Testing
1. 创建一个 Game Mode Base 蓝图
2. Game Mode Base 蓝图 设置 Default Pawn Class 为创建的 Character 蓝图
3. 在 World Settings 的 Game Mode 下的 GameMode Override 设置为新创建的 Game Mode Base 蓝图
4. 在 Project Settings 下的 Maps & Modes 下的 Default Modes 下的 Default GameMode 设置为新创建的 Game Mode Base 蓝图

### 15 - Character Gamepad & Touch Controls
略

### 16 - Intro to Animation Montage
略

### 17 - Skeleton Retargeting & Montage Setup
### 18 - Animation BP Punching Setup
### 19 - Playing Our Animation Montage
### 20 - Using Slot Nodes & Branch Points
### 21 - Add Physics Components for Punching
### 22 - Creating Animation Notifies

这个看完，看以前那个横板动画设置
1. 绑模型到角色骨骼
2. 增加插槽
3. 两个动作混合
4. 蒙太奇动作
5. 走和跑的切换
6. 动画事件通知
