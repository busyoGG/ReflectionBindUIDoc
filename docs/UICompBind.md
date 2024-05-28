# UI 组件绑定

## 使用方法

在 UI 类中，在对应组件属性上添加一个特性标签。标签由两个参数组成：

1. UI 类型。
2. UI 路径。

```C# 
//UI类型为Loader，绑定根界面的索引为3的元素
[UICompBind(UIType.Loader, "3")]
private GLoader _loader { get; set; }
```

## 类型表

|类型 | 枚举 |
|:-:|:-:|
|组件|UIType.Comp|
|文本|UIType.TextField|
|输入框|UIType.TextInput|
|装载器|UIType.Loader|
|列表|UIType.List|
|滑动条|UIType.Slider|
|图片|UIType.Image|
|下拉框|UIType.ComboBox|