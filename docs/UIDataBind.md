# UI 数据绑定

## 使用方法

在 UI 类中，在对应组件属性上添加一个特性标签。标签由三个参数组成：

1. UI 类型。
2. UI 路径。

```C# 
//UI类型为TextField，绑定根界面的索引为0的元素
[UIDataBind(UIType.TextField, "0")]
private StringUIProp _testText { get; set; }
```

## 类型表

|类型 | 枚举 | 参数类型 | 辅助特性 |
|:-:|:-:|:-:|:-:|
|组件|UIType.Comp|None||
|文本|UIType.TextField|StringUIProp||
|输入框|UIType.TextInput|StringUIProp||
|装载器|UIType.Loader|StringUIProp||
|列表|UIType.List|ListUIProp|UIAdaptive：用于自适应宽高 |
|滑动条|UIType.Slider|DoubleUIProp||
|图片|UIType.Image|StringUIProp||
|下拉框|UIType.ComboBox|DoubleUIProp(索引)|UIOptions：选项内容|