# UI 类绑定

## 使用方法

在 UI 类上添加一个特性标签。标签由一个参数组成：

1. 类绑定类型。

```C# 
[UIClassBind(UIClass.Drag),UIWindow(true,"n10"),UIColor(1,1,1,0.5f)]
public class TestView : BaseView
```

## 类型表

|类型 | 枚举 | 辅助特性 |辅助说明 |
|:-:|:-:|:-:|:-:|
|模态窗口|UIClass.Model|1.UICondition(bool) <br>2.UIColor |1.是否允许点击模态背景时隐藏窗口<br>2.模态窗口颜色 |
|拖拽|UIClass.Drag|UIWindow|是否置顶选中窗口，哪个部分的 UI 允许拖拽窗口|
