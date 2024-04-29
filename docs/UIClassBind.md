# UI类绑定

## 使用方法

在UI类上添加一个特性标签。标签由两个参数组成：

1. 类绑定类型。
2. 额外数据。

```C# 
[UIClassBind(UIClass.Drag,"Retop","n10"),UIColor(1,1,1,0.5f)]
public class TestView : BaseView
```

## 类型表

|类型|枚举|额外参数|
|:-:|:-:|:-:|
|模态窗口|Model|Hide:点击模态背景，隐藏窗口|
|拖拽|Drag|两种情况：<br>整体UI判定拖拽 -> 1. Retop:重新排序<br>部分UI判定拖拽 -> 1. Retop:重新排序 <br>2. *：拖拽部分UI路径|

## 特殊情况

### 模态窗口

模态窗口可以额外传入一个 `UIColor` 标签来设置模态背景颜色。默认不传入则为透明背景。

`UIColor` 有四个参数 `r,g,b,a`，对应 `Color` 的四个分量。