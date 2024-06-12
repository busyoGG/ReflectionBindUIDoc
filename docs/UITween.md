# 窗口缓动动画

## 使用方法

### 创建缓动

#### 界面缓动

为了创建界面缓动，我们需要使用 `AddTween` 来添加缓动。

```C#
//缓动目标为X坐标，移动距离为正500，持续时间2000毫秒，插值函数为CircOut
AddTween(TweenTarget.X,500,2000,TweenEaseType.CircOut);
```

`AddTween` 有 5 个参数：

|   参数   |                                               描述                                                |
| :------: | :-----------------------------------------------------------------------------------------------: |
|  target  |                                             缓动目标                                              |
|   end    | 目标值，缓动对象从原值经过**目标值**到达新的值 <br>目标值有两种类型，一种是 float，一种是 Vector2 |
| duration |                                        持续时间，单位毫秒                                         |
|   ease   |                    插值函数，决定每帧插值的比例，可以实现线性或由快到慢等效果                     |
| callback |                                     回到函数，缓动结束后触发                                      |

#### UI 元素缓动

为了创建 UI 元素缓动，我们需要使用 `AddTween` 来添加缓动。

```C#
GObject obj;
//缓动目标为X坐标，移动距离为正500，持续时间2000毫秒，插值函数为CircOut
AddTween(obj,TweenTarget.X,500,2000,TweenEaseType.CircOut);
```

`AddTween` 有 6 个参数：

|   参数   |                                               描述                                                |
| :------: | :-----------------------------------------------------------------------------------------------: |
|   obj    |                                             缓动主体                                              |
|  target  |                                             缓动目标                                              |
|   end    | 目标值，缓动对象从原值经过**目标值**到达新的值 <br>目标值有两种类型，一种是 float，一种是 Vector2 |
| duration |                                        持续时间，单位毫秒                                         |
|   ease   |                    插值函数，决定每帧插值的比例，可以实现线性或由快到慢等效果                     |
| callback |                                     回到函数，缓动结束后触发                                      |

### 打开窗口

打开窗口有两个重载函数：

1. 缓动动画初始化函数。
2. 缓动回调函数。

```C# 初始化
protected override void TweenIn()
{
    main.x = -500;
    AddTween(TweenTarget.X,500,2000,TweenEaseType.CircOut);

    main.y = 500;
    AddTween(TweenTarget.Y,-500,2000,TweenEaseType.Linear);

    main.scaleX = 0.5f;
    AddTween(TweenTarget.ScaleX,0.5f,2000,TweenEaseType.CircOut);

    AddTween(TweenTarget.Rotation,50,2000,TweenEaseType.CircOut);
}
```

```C# 回调
protected override void OnShow()
{
    ConsoleUtils.Log("缓动回调");
}
```

根据需要重载即可。

### 关闭窗口

关闭窗口和打开窗口类似，也有两个重载函数：

```C# 初始化
protected override void TweenOut()
{
    AddTween(TweenTarget.X,500,2000,TweenEaseType.CircOut);
}
```

```C# 回调
protected override void OnHide()
{
    ConsoleUtils.Log("缓动回调");
}
```

## 类型表

|   类型   |                   描述                   |
| :------: | :--------------------------------------: |
|   None   | 无缓动，主要是内部使用，对齐缓动回调时机 |
|    X     |                X 坐标缓动                |
|    Y     |                Y 坐标缓动                |
| Position |               XY 坐标缓动                |
|   Size   |                 宽高缓动                 |
|  ScaleX  |                X 缩放缓动                |
|  ScaleY  |                Y 缩放缓动                |
|  Scale   |               XY 缩放缓动                |
| Rotation |                 旋转缓动                 |
|  Alpha   |                透明度缓动                |
|  Heihgt  |                 高度缓动                 |
|  Width   |                 宽度缓动                 |
