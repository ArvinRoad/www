---
title: Lua讲解
date: 2021/8/29 01:02:00
tags:
  - Lua
  - C++
  - 程序开发
  - 教学文档
categories:
  - 程序 
cover: https://pic4.zhimg.com/v2-149560fe9bf1888aad0035c490f6a59c_1440w.jpg?source=172ae18b
copyright_author: Forkelous Alen（程序负责人）
copyright_author_href: https://github.com/ForkelousAlen
copyright_info: 此文章版權歸Forkelous Alen所有，如有轉載，請註明來自原作者
---

# 概述
UnLua是一个功能丰富并且高效的UE4脚本编程解决方案。开发者可以使用Lua来开发游戏逻辑，并且它允许我们利用Lua的热加载功能来更快地更新游戏逻辑。这份文档将会介绍UnLua的主要功能以及最基础的编程模式。

---

# Lua和引擎的绑定
UnLua提供了一种简单的方法将Lua和游戏引擎相互绑定，包括静态绑定和动态绑定：

## 静态绑定

#### C++
你的UCLASS只需要实现接口**IUnLuaInterface**，并在函数**GetModuleName_Implementation()**中返回一个Lua文件路径。

#### 蓝图
你的蓝图只需要实现接口**UnLuaInterface**，并在函数**GetModuleName()**中返回一个Lua文件路径。

## 动态绑定
动态绑定适用于运行时创建AActor和UObject

#### Actor
``` lua
local Proj = World:SpawnActor(ProjClass, Transform, ESpawnActorCollisionHandlingMethod.AlwaysSpawn, self, self.Instigator, "Weapon.BP_DefaultProjectile_C")
```
**“Weapon.BP_DefaultProjectile_C”**是一个Lua文件路径.

#### Object
``` lua
local ProxyObj = NewObject(ObjClass, nil, nil, "Objects.ProxyObject")
```
**“Objects.ProxyObject”**是一个Lua文件路径.

## Lua文件路径
不论是静态绑定还是动态绑定都需要一个Lua文件路径。它是项目目录**'Content/Script'**的**相对**路径。

---

# Lua调用引擎
UnLua提供了两种从Lua端访问引擎的方法：
- 使用反射系统动态导出；
- 在反射系统外部静态导出类、成员变量、成员函数、全局函数和枚举。

## 使用反射系统动态导出
利用反射系统进行动态导出，使代码简洁、直观，消除了大量的胶水代码。

### 访问UCLASS
``` lua
local Widget = UWidgetBlueprintLibrary.Create(self, UClass.Load("/Game/Core/UI/UMG_Main"))
```
**UWidgetBlueprintLibrary** 是一个UCLASS。类在Lua里的名称必须是 **PrefixCPP** + **ClassName** + **[``_C``]**，例如： **AActor** (原生类)， **ABP_PlayerCharacter_C**（蓝图类）

### 访问UFUNCTION
``` lua
Widget:AddToViewport(0)
```
**AddToViewport** 是 **UUserWidget** 的一个UFUNCTION。 **0** 是函数的参数。如果（被标记为 **'BlueprintCallable'** 或 **'Exec'**的）UFUNCTION的参数拥有默认值，那在Lua代码中可以忽略参数0：
``` lua
Widget:AddToViewport()
```

#### 输出值处理
输出值包括 **非常量引用参数** and **返回值参数**。这些输出值分为 **原生类型（bool, integer, number, string）** 和 **非原生类型 （用户自定义数据）**。

##### 非常量引用参数
###### 原生类型

Lua代码：
``` lua
local Level, Health, Name = self:GetPlayerBaseInfo()
```

###### 非原生类型

他们在Lua中有两种调用的方法：
``` lua
local HitResult = FHitResult()
self:GetHitResult(HitResult)
```
或
``` lua
local HitResult = self:GetHitResult()
```
第一种方法和C++极为相似，当调用多次（比如在循环中）时，它比第二种方法效率高得多。

##### 返回值参数
###### 原生类型

Lua代码：
``` lua
local MeleeDamage = self:GetMeleeDamage()
```

###### 非原生类型

他在Lua中有三种调用方式：
``` lua
local Location = self:GetCurrentLocation()
```
或者
``` lua
local Location = FVector()
self:GetCurrentLocation(Location)
```
以及
``` lua
local Location = FVector()
local LocationCopy = self:GetCurrentLocation(Location)
```
第一种方法最为直观，事实上，当调用多次（比如在循环中）时，后两种方法要比第一种方法效率高得多。最后一种方法等价于：
``` lua
local Location = FVector()
self:GetCurrentLocation(Location)
local LocationCopy = Location
```

#### 潜在功能
潜在功能允许开发人员使用同步调用风格开发异步逻辑。一个典型的潜在功能例子是**Delay**：

你可以在Lua协程中调用潜在功能：
``` lua
coroutine.resume(
	coroutine.create(
    	function(GameMode, Duration)
        	UKismetSystemLibrary.Delay(GameMode, Duration)
        end
    ), self, 5.0
)
```

#### 优化
UnLua对UFUNCTION的调用进行了以下几点优化：

 * 持久的缓冲参数
 * 优化局部函数调用
 * 优化参数传递
 * 优化输出值处理

### 访问USTRUCT
``` lua
local Position = FVector()
```
**FVector** 是一个USTRUCT。

### 访问UPROPERTY
``` lua
local Position = FVector()
Position.X = 256.0
```
**X** 是 **FVector** 的一个UPROPERTY。

#### 代理
* 绑定
``` lua
FloatTrack.InterpFunc:Bind(self, BP_PlayerCharacter_C.OnZoomInOutUpdate)
```
**InterpFunc** 是 FTimelineFloatTrack 的代理， **'Bind'** 为 **InterpFunc** 绑定了一个回调函数 （**BP_PlayerCharacter_C.OnZoomInOutUpdate**）。

* 解除绑定
``` lua
FloatTrack.InterpFunc:Unbind()
```
**InterpFunc** 是 FTimelineFloatTrack 的代理， **'Unbind'** 解除了 **InterpFunc** 所绑定的回调函数。

* 执行
``` lua
FloatTrack.InterpFunc:Execute(0.5)
```
**InterpFunc** 是 FTimelineFloatTrack 的代理， **'Execute'** 调用了绑定到**InterpFunc**对象上的函数。


#### 多播代理
* 添加
```
 self.ExitButton.OnClicked:Add(self, UMG_Main_C.OnClicked_ExitButton)
```
**OnClicked** 是 UButton 的一个多播代理，**'Add'** 为 **OnClicked** 添加了一个回调（**UMG_Main_C.OnClicked_ExitButton**）。

* 移除
```
 self.ExitButton.OnClicked:Remove(self, UMG_Main_C.OnClicked_ExitButton)
```
**OnClicked** 是 UButton 的一个多播代理，**'Remove'** 在 **OnClicked** 上移除了一个回调（**UMG_Main_C.OnClicked_ExitButton**）。

* 清除
```
 self.ExitButton.OnClicked:Clear()
```
**OnClicked** 是 UButton 的一个多播代理，**'Clear'** 清除了在 **OnClicked** 上的所有回调。

* 广播
```
 self.ExitButton.OnClicked:Broadcast()
```
**OnClicked** 是 UButton 的一个多播代理，**'Broadcast'** 调用了所有绑定在 **OnClicked** 对象上的函数。

### 访问UENUM
``` lua
Weapon:K2_AttachToComponent(Point, nil, EAttachmentRule.SnapToTarget, EAttachmentRule.SnapToTarget, EAttachmentRule.SnapToTarget)
```
**EAttachmentRule** 是一个枚举，**SnapToTarget** 是一个 **EAttachmentRule** 类型的枚举值。

#### 自定义碰撞枚举

* EObjectTypeQuery
``` lua
local ObjectTypes = TArray(EObjectTypeQuery)
ObjectTypes:Add(EObjectTypeQuery.Player)
ObjectTypes:Add(EObjectTypeQuery.Enemy)
ObjectTypes:Add(EObjectTypeQuery.Projectile)
local bHit = UKismetSystemLibrary.LineTraceSingleForObjects(self, Start, End, ObjectTypes, false, nil, EDrawDebugTrace.None, HitResult, true)
```
**EObjectTypeQuery.Player**，**EObjectTypeQuery.Enemy** 以及 **EObjectTypeQuery.Projectile** 都是自定义对象通道。

* ETraceTypeQuery
``` lua
local bHit = UKismetSystemLibrary.LineTraceSingle(self, Start, End, ETraceTypeQuery.Weapon, false, nil, EDrawDebugTrace.None, HitResult, true)
```
**ETraceTypeQuery.Weapon** 是一个自定义跟踪通道。

### 手动导出库
出于灵活性与性能考虑，UnLua在引擎中手动导出了几个重要的类，包括以下（详见代码）：

#### 基础类
 * UObject
 * UClass
 * UWorld
 
#### 常见容器
 * TArray
 * TSet
 * TMap
 
##### 示例
``` lua
	local Indices = TArray(0)
	Indices:Add(1)
	Indices:Add(3)
	Indices:Remove(0)
	local NbIndices = Indices:Length()
```
``` lua
	local Vertices = TArray(FVector)
	local Actors = TArray(AActor)
```

#### 数学库
 * FVector
 * FVector2D
 * FVector4
 * FQuat
 * FRotator
 * FTransform
 * FColor
 * FLinearColor
 * FIntPoint
 * FIntVector

## 静态导出
UnLua提供了一个简单的解决方案，可以在反射系统外部静态地导出类、成员变量、成员函数、全局函数和枚举。

### 类
* 没有反射的类
``` cpp
BEGIN_EXPORT_CLASS(ClassType, ...)
```
或者
``` cpp
BEGIN_EXPORT_NAMED_CLASS(ClassName, ClassType, ...)
```
 **'...'** 表示构造函数中参数的类型列表。

* 有反射的类
``` cpp
BEGIN_EXPORT_REFLECTED_CLASS(UObjectType)
```
或者
``` cpp
BEGIN_EXPORT_REFLECTED_CLASS(NonUObjectType, ...)
```
 **'...'** 表示构造函数中参数的类型列表。

#### 成员变量
``` cpp
ADD_PROPERTY(Property)
```
或者（用于bitfield的bool属性）
``` cpp
ADD_BITFIELD_BOOL_PROPERTY(Property)
```

#### 成员函数
##### 非静态成员函数
* 紧凑型风格
``` cpp
ADD_FUNCTION(Function)
```
或者
``` cpp
ADD_NAMED_FUNCTION(Name, Function)
```

* 完全型风格
``` cpp
ADD_FUNCTION_EX(Name, RetType, Function, ...)
```
 或者
``` cpp
ADD_CONST_FUNCTION_EX(Name, RetType, Function, ...)
```
 **'...'** 为参数类型列表。

##### 静态成员函数
``` cpp
ADD_STATIC_FUNCTION(Function)
```
或者
``` cpp
ADD_STATIC_FUNCTION_EX(Name, RetType, Function, ...)
```
**'...'** 为参数类型列表。

#### 示例
``` cpp
struct Vec3
{
	Vec3() : x(0), y(0), z(0) {}
	Vec3(float _x, float _y, float _z) : x(_x), y(_y), z(_z) {}

	void Set(const Vec3 &V) { *this = V; }
	Vec3& Get() { return *this; }
	void Get(Vec3 &V) const { V = *this; }

	bool operator==(const Vec3 &V) const { return x == V.x && y == V.y && z == V.z; }

	static Vec3 Cross(const Vec3 &A, const Vec3 &B) { return Vec3(A.y * B.z - A.z * B.y, A.z * B.x - A.x * B.z, A.x * B.y - A.y * B.x); }
	static Vec3 Multiply(const Vec3 &A, float B) { return Vec3(A.x * B, A.y * B, A.z * B); }
	static Vec3 Multiply(const Vec3 &A, const Vec3 &B) { return Vec3(A.x * B.x, A.y * B.y, A.z * B.z); }

	float x, y, z;
};

BEGIN_EXPORT_CLASS(Vec3, float, float, float)
	ADD_PROPERTY(x)
	ADD_PROPERTY(y)
	ADD_PROPERTY(z)
	ADD_FUNCTION(Set)
	ADD_NAMED_FUNCTION("Equals", operator==)
	ADD_FUNCTION_EX("Get", Vec3&, Get)
	ADD_CONST_FUNCTION_EX("GetCopy", void, Get, Vec3&)
	ADD_STATIC_FUNCTION(Cross)
	ADD_STATIC_FUNCTION_EX("MulScalar", Vec3, Multiply, const Vec3&, float)
	ADD_STATIC_FUNCTION_EX("MulVec", Vec3, Multiply, const Vec3&, const Vec3&)
END_EXPORT_CLASS()
IMPLEMENT_EXPORTED_CLASS(Vec3)
```

### 全局函数
``` cpp
EXPORT_FUNCTION(RetType, Function, ...)
```
或者
```
EXPORT_FUNCTION_EX(Name, RetType, Function, ...)
```
**'...'** 为参数类型列表。

#### 示例
``` cpp
void GetEngineVersion(int32 &MajorVer, int32 &MinorVer, int32 &PatchVer)
{
	MajorVer = ENGINE_MAJOR_VERSION;
	MinorVer = ENGINE_MINOR_VERSION;
	PatchVer = ENGINE_PATCH_VERSION;
}

EXPORT_FUNCTION(void, GetEngineVersion, int32&, int32&, int32&)
```

### 枚举
* 普通枚举

``` cpp
enum EHand
{
	LeftHand,
	RightHand
};

BEGIN_EXPORT_ENUM(EHand)
	ADD_ENUM_VALUE(LeftHand)
	ADD_ENUM_VALUE(RightHand)
END_EXPORT_ENUM(EHand)
```

* 作用域为类的枚举

``` cpp
enum class EEye
{
	LeftEye,
	RightEye
};

BEGIN_EXPORT_ENUM(EEye)
	ADD_SCOPED_ENUM_VALUE(LeftEye)
	ADD_SCOPED_ENUM_VALUE(RightEye)
END_EXPORT_ENUM(EEye)
```

## 可选“UE4”名称空间
UnLua提供了一个选项来添加一个命名空间**'UE4'**到引擎中的所有类和枚举。你可以在**UnLua.Build.cs**中找到这个选项。

如果这个选项被开启，你的Lua代码应该是这样子：
``` lua
local Position = UE4.FVector()
```

---

# 引擎调用Lua
UnLua提供了一个类似蓝图的解决方案来跨越C++/Script的边界。它允许C++/Blueprint代码调用在Lua代码中定义的函数。

## 覆写蓝图事件
你可以在Lua代码里覆写所有的**蓝图事件**。**蓝图事件**包括：

* 被标记为 **'BlueprintImplementableEvent'** 的UFUNCTION
* 被标记为 **'BlueprintNativeEvent'** 的UFUNCTION
* **所有**定义在蓝图里的事件或函数

### 示例（没有返回值的蓝图事件）

你可以在Lua中这样覆写它：
``` lua
function BP_PlayerController_C:ReceiveBeginPlay()
  print("ReceiveBeginPlay in Lua!")
end
```

### 示例（有返回值的蓝图事件）

在Lua中有两种覆写它的方式：
``` lua
function BP_PlayerCharacter_C:GetCharacterInfo(HP, Position, Name)
	Position.X = 128.0
	Position.Y = 128.0
	Position.Z = 0.0
	return 99, nil, "Marcus", true
end
```
或者
``` lua
function BP_PlayerCharacter_C:GetCharacterInfo(HP, Position, Name)
	return 99, FVector(128.0, 128.0, 0.0), "Marcus", true
end
```
推荐第一种方式。

## 覆写动画通知
动画通知：

Lua代码：
``` lua
function ABP_PlayerCharacter_C:AnimNotify_NotifyPhysics()
	UBPI_Interfaces_C.ChangeToRagdoll(self.Pawn)
end
```
Lua的函数名称必须是 **'AnimNotify_'** + **通知名称**。

## 覆写输入事件

#### 操作输入
``` lua
function BP_PlayerController_C:Aim_Pressed()
	UBPI_Interfaces_C.UpdateAiming(self.Pawn, true)
end

function BP_PlayerController_C:Aim_Released()
	UBPI_Interfaces_C.UpdateAiming(self.Pawn, false)
end
```
Lua的函数名称必须是 **操作输入名称** + **'_Pressed' 或者 '_Released'**。

#### 轴输入
``` lua
function BP_PlayerController_C:Turn(AxisValue)
	self:AddYawInput(AxisValue)
end

function BP_PlayerController_C:LookUp(AxisValue)
	self:AddPitchInput(AxisValue)
end
```
Lua的函数名称必须是与 **轴输入名称** 一模一样。

#### 键盘输入
``` lua
function BP_PlayerController_C:P_Pressed()
	print("P_Pressed")
end

function BP_PlayerController_C:P_Released()
	print("P_Released")
end
```
Lua的函数名称必须是 **按键名称** + **'_Pressed' 或者 '_Released'**。

#### 其他输入
你也可以在Lua中覆写 **Touch/AxisKey/VectorAxis/Gesture Inputs**。

## 覆写复制通知
如果你正在开发专用/侦听服务器&客户端游戏，你可以在Lua代码中覆写复制通知：

``` lua
function BP_PlayerCharacter_C:OnRep_Health(...)
	print("call OnRep_Health in Lua")
end
```

## 调用被覆写的函数
如果你在Lua中覆写了一个蓝图事件、动画通知或者复制通知，你仍然可以访问原始函数实现。
``` lua
function BP_PlayerController_C:ReceiveBeginPlay()
	local Widget = UWidgetBlueprintLibrary.Create(self, UClass.Load("/Game/Core/UI/UMG_Main"))
	Widget:AddToViewport()
	self.Overridden.ReceiveBeginPlay(self)
end
```

**self.*Overridden*.ReceiveBeginPlay(self)** 将会调用蓝图实现的 'ReceiveBeginPlay'。

## 在C++中调用Lua函数
UnLua还提供了两种通用方法来调用全局Lua函数和C++代码中全局Lua表中的函数。

* 全局函数
``` cpp
template <typename... T>
FLuaRetValues Call(lua_State *L, const char *FuncName, T&&... Args);
```

* 全局表中的函数
``` cpp
template <typename... T>
FLuaRetValues CallTableFunc(lua_State *L, const char *TableName, const char *FuncName, T&&... Args);
```

---

# 其他

* Lua模板文件导出

你可以在蓝图中生成Lua的模板文件

模板文件：


