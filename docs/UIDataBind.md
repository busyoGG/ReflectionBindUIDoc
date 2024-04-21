# UI数据绑定

## 使用方法

在UI类中，在对应组件属性上添加一个特性标签。标签由三个参数组成：

1. UI类型。
2. UI路径。
3. 额外参数。

```C# 
//UI类型为TextField，绑定根界面的索引为0的元素
[UIDataBind(UIType.TextField, "0")]
private StringUIProp _testText { get; set; }
```

## 类型表

|类型|枚举|额外参数|
|:-:|:-:|:-:|
|组件|Comp||
|文本|TextField||
|输入框|TextInput||
|装载器|Loader||
|列表|List|仅用于同类型Item <br>height:自适应高度 <br> width:自适应宽度|
|滑动条|Slider||
|图片|Image||