## [Blueprint 3rd Person Game]

[教学视频首页](https://docs.unrealengine.com/latest/INT/Videos/PLZlv_N0_O1ga0IoRrpI4xkX4qmCrhGu56/hRO82u1phyw/index.html)

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
#### 骨骼错位处理
1. 如果导入的骨骼有错位现象，在 Skeleton Tree，点右键选择：Recursively Set Translation Retargeting Skeleton
2. 执行上一步后，会出现一个根节点上移现象，对 pelvis 骨骼的 Translation Retargeting 设置为 ： Animation Scaled
3. 对 root 骨骼的 Translation Retargeting 设置为 ： Animation

#### Montage 操作基础
1. Montage 是对 Animation Sequence 的再加工，比 Animation Sequence 多了 Slot ？？？
2. Montage Group 由 Slot 组成
3. Slot 由 Sections 组成
4. Slot 的作用是只用于动画混合？？？
5. 在 Montage 操作界面的 Section 部分，可以放入任意多个Animation Sequence
6. 在 Section 部分，可以给一个 Animation Sequence 打上若干个Section标签
7. 也可以给多个 Animation Sequence 打上一个标签
8. 在 Montage 操作界面的 Sections 部分，可以把任意的 Section 组成一个播放序列
9. 实际动画播放，就是播放这个序列

#### Montage 播放动画跳转
1. 教学中用 Montage 做的打拳，分为启动、打、收拳，打分为左右拳，收拳分为左右收拳，对应左右出拳，总共5个 Animation Sequence ？？？
2. 因为打拳是按键控制，为了确定收拳用那个动画，在 Montage 操作界面的 Notifies 部分 增加两个事件，分别在左右出拳 Animation Sequence 的快结束部分
3. 在动画蓝图里，监听增加的事件，如果用户松开出拳按键，在那个事件区域，就跳转到对应的收拳 Animation Sequence

#### 设置Slot
1. 在 Anim Slot Manager 里增加新的 Slot
2. 在 Montage Group 部分，设置 Slot 为新增加的 Slot
3. 这个 Slot 主要是标记 Montage？？？方便后面在 CharacterAnim 蓝图里和别的动画混合？？？

### 18 - Animation BP Punching Setup
这个主要讲在 Character 蓝图 和 CharacterAnim 蓝图 根据用户的按键 设置是否出拳的变量

### 19 - Playing Our Animation Montage
#### 播放 Montage
1. 在动画蓝图的 Event Graph 部分，增加一个 新事件【Punch】
2. 在 Event Blueprint Update Animation 里检测玩家是否按了打拳按键
3. 如果玩家按了打拳按键，调用 DoOnce 调用一次 Punch 事件
4. Punch 事件，调用 Montage Play 播放打拳

#### 处理 Montage 的 Notifies 事件
1. 在动画蓝图的 Event Graph 部分，把 【17】 课增加的两个事件通知拖入
2. 根据玩家是否打拳的变量判断是否收拳
3. 如果收拳，那么根据对应的事件通知，调用 Montage Set Next Section 跳转到对应的收拳动画

### 20 - Using Slot Nodes & Branch Points
1. 在  CharacterAnim 蓝图，Anim Graph 的操作区域，点右键，选择从弹出菜单 Cached Poses 下的子菜单 New Save Cached Pose
2. 把新增加的蓝图节点和状态机节点相连，缓存状态机，以便重复使用状态机数据
3. 点右键，选择从弹出菜单 Cached Poses 下的子菜单 Use Cached Pose，重复两次
4. 点右键，选择从弹出菜单 Montage 下的子菜单 Slot 'DefaultSlot'
5. 选择新增的 Slot 'DefaultSlot' 节点，在 Details 面板下，选择 Slot Name 为 17 课设置的 Slot 名
6. 点右键，选择从弹出菜单 Blends 下的子菜单 Layered blend per bone
7. 把两个 Use Cached Pose 节点和一个 Slot 节点与 Layered blend per bone 节点相连接
8. 选择新增的 Layered blend per bone 节点，在 Details 面板下，找到 Bone Name 设置为开始混合时的起始节点

#### 总结
1. 这个主要实现了两个动画的混合播放，让一个角色同时做两套动画，一套为 Base Pose，一套是根据设置的 Bone Name，Bone Name对应骨骼以上部分做另一套动画
2. Base Pose 基本为：由跑、跳、空闲等基本动作构成的 State Machine
3. 作为 Blend 的另一套动画，基本是 Montage 动画，比如打拳
4. 这个 Blend 处理后，可以做出一边跑一边打拳，一边跳一边打拳
5. 大概处理流程是：由事件触发【比如按键】 Montage 动画，Montage 动画播放结束回到 Base Pose 设置的动画

### 21 - Add Physics Components for Punching
这个教学的知识点可以应用到拳打脚踢实现

1. 在 Character 蓝图里的 ViewPort 操作界面，在手部的位置增加两个圆形碰撞体
2. 选择两个圆形碰撞体，在 Details 面板下的 Collision 部分，把 Collision Presets 设置为：NoCollision【这个目的就是平时无碰撞，只有事件触发才有碰撞】
3. 在 Character 蓝图里的 Construction Script 操作界面，用 AttachToComponent 函数，把两个圆形碰撞体分别绑定到左右手的骨骼点
4. 在 Character 蓝图里的 Event Graph 操作界面，在处理出拳按键部分，调用 Set Collision Enabled 函数，当出拳按键按下设置能碰撞，当出拳按键松开设置不能碰撞

### 22 - Creating Animation Notifies
1. 在 Animation 动画操作面板，在 Notifies 操作区，点右键，会弹出 Notify 菜单，点 Add Notify 下的 Play Particle Effect，增加粒子效果
2. 在 Notifies 操作区，选择新增加的粒子标识，在 Details 面板，通过 Socket Name 设置粒子效果，绑定到角色的那个骨骼或插槽
3. 在 Details 面板，Attached 用来设置粒子是否随骨骼一起运动

### 额外知识点
拖拽 Character 蓝图到场景，在 Details 面板下的 Pawn 组里，设置 Auto Possess Player 为 Player 0，这样运行项目，拖入场景的 Character 蓝图为控制 Character，可以把 Player Start 去掉。

这个看完，看以前那个横板动画设置
1. 绑模型到角色骨骼
2. 增加插槽
3. 两个动作混合
4. 蒙太奇动作
5. 走和跑的切换
6. 动画事件通知
