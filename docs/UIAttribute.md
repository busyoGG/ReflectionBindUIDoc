# 辅助特性

## UICondition

条件判断。传入一个参数。

### 使用方法

```C# 
[UIActionBind(UIAction.DragEnd, "2"),UICondition(false)]
private void OnDrag(EventContext context)
{
    //添加拖拽数据
    AddDropData(1, 2, "测试数据");
    Debug.Log("结束拖拽");
}
```

### 参数说明

|构造函数参数类型 |调用方法 |
|:-:|:-:|
|string|`GetString()`|
|int|`GetInt()`|
|bool|`GetBool()`|
|float|`GetFloat()`|
|double|`GetDouble()`|

## UIAdaptive

自适应宽高。传入一个参数。

### 使用方法

```C# 
[UIDataBind(UIType.List, "n1"),UIAdaptive(AdaptiveType.Height)]
private UIListProp<string> _testList { get; set; }
```

### 参数说明

|参数类型 |说明 |
|:-:|:-:|
|AdaptiveType.Width|宽度自适应 |
|AdaptiveType.Height|高度自适应 |
|AdaptiveType.All|宽高自适应 |   

## UIOptions

选项数组。传入若干个参数。

### 使用方法

```C# 
[UIDataBind(UIType.ComboBox,"9"),UIOptions("1","a","选项3")]
private DoubleUIProp _comboBoxIndex { get; set; }
```

### 参数说明


|参数类型 |说明 |
|:-:|:-:|
|params string[]|选项|

## UIWindow

窗口属性。传入两个参数。

### 使用说明

```C# 
[UIClassBind(UIClass.Drag),UIWindow(true,"n10")]
public class TestView : BaseView{
    //...
}
```

### 参数说明

|参数类型 |说明 |
|:-:|:-:|
|bool|是否置顶窗口 |
|string|UI 元素路径，允许通过该 UI 元素拖拽窗口，不传就全窗口区域可拖拽|

## UIColor

颜色。传入四个参数。

### 使用说明

```C# 
[UIClassBind(UIClass.Model),Condition(true),UIColor(1,1,1,0.5f)]
public class TestView : BaseView{
    //...
}
```

### 参数说明

|参数类型 |说明 |
|:-:|:-:|
|float|r 颜色分量 |
|float|g 颜色分量 |
|float|b 颜色分量 |
|float|a 透明度分量|