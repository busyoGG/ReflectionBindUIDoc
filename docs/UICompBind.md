# UI组件绑定

## 使用方法

在UI类中，在对应组件属性上添加一个特性标签。标签由两个参数组成：

1. UI类型。
2. UI路径。

```C# 
//UI类型为Loader，绑定根界面的索引为3的元素
[UICompBind(UIType.Loader, "3")]
private GLoader _loader { get; set; }
```

## 类型表

|类型|枚举|
|:-:|:-:|
|组件|Comp|
|文本|TextField|
|输入框|TextInput|
|装载器|Loader|
|列表|List|
|滑动条|Slider|
|图片|Image|