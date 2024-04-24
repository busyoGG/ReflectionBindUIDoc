# 快速开始

## 简介

利用特性Attribute来反射初始化UI组件，使其和数据进行双向绑定，更新数据的时候同步更新UI，更新UI的时候同步更新数据（只更新当前界面定义的数据，原始数据需要手动更新）。

在此基础上拓展了UI动作绑定和UI监听绑定，这样只需要对方法标记特性，就可以自动完成对应的绑定工作。

基本上每一个功能和属性都只专注于其自身，只有在需要手动获取某个UI元素并且进行操作的时候会在实现代码中产生交叉。

## 基本用法

### 使用方式

基本用法请见导航栏UI绑定部分内容。

所有绑定的数据都必须是 `StringUIProp`、`DoubleUIProp`、`UIListProp` 其中一种。

其中 `UIListProp` 保存的数据是 `List<T>` 类型，数据更新的时候只更新 UI 列表元素的渲染数量，即 `numItems`。渲染函数对应索引的数据请自行获取。

### UI获取

UI元素路径，即 path ，可以通过两种方式设置。

1. 索引数字。
2. UI名字。

虽然有两种获取 UI 的方式，但是传入的数据都是 `string` 类型，索引也不例外。

### 更新数据

本项目的数据和UI是双向绑定的。

只需要使用Set方法设置数据即可，UI自己会更新。

```C#
//设置了装载器的图片路径，装载器组件会自动设置src="ui://Test/Icon"
_loaderUrl.Set("ui://Test/Icon");
```

如果UI更新了数据，则通过Get方法获取的时候会自动更新。

```C#
//获取参数的值
_loaderUrl.Get();
```

> 注意：UI监听使用 [事件管理器](https://github.com/busyoGG/EventManagerForUnity) 项目的事件系统，如果需要使用自己的事件监听方法请参考导航栏中的拓展选项。

## 案例

```C# 
using FairyGUI;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TestView : BaseView
{
    [UIDataBind(UIType.TextField, "0")]
    private StringUIProp _testText { get; set; }

    [UIDataBind(UIType.List, "n1")]
    private UIListProp<string> _testList { get; set; }

    [UIDataBind(UIType.Loader, "3")]
    private StringUIProp _loaderUrl { get; set; }

    [UIDataBind(UIType.Slider, "4")]
    private DoubleUIProp _slideValue { get; set; }

    [UIDataBind(UIType.TextInput, "5")]
    private StringUIProp _input { get; set; }

    [UICompBind(UIType.Loader, "3")]
    private GLoader _loader { get; set; }


    [UIActionBind(UIAction.ListRender, "1")]
    private void ItemRenderer(int index, GObject item)
    {
        GComponent comp = item as GComponent;
        GTextField content = comp.GetChildAt(0).asTextField;

        content.text = _testList.Get()[index];

        SetDrag(UIAction.DragStart, comp, () =>
        {
            ConsoleUtils.Log("开始拖拽");
            AddDropData(index);
        });

        SetDrop(comp, (object data) => { ConsoleUtils.Log("放置", data); });
    }

    [UIActionBind(UIAction.Click, "2")]
    private void OnBtnClick()
    {
        ConsoleUtils.Log("点击了按钮");
        EventManager.TriggerEvent("show_console", null);
    }

    [UIActionBind(UIAction.DragEnd, "2", "Self")]
    private void OnDrag(EventContext context)
    {
        //添加拖拽数据
        AddDropData(1, 2, "测试数据");
        Debug.Log("结束拖拽");
        Debug.Log(GRoot.inst.touchTarget);
    }

    [UIActionBind(UIAction.Drop, "3")]
    private void OnDrop(object data)
    {
        ConsoleUtils.Log("拖拽放置", data);
    }

    [UIListenerBind("show_console")]
    private void ShowConsole(ArrayList data)
    {
        ConsoleUtils.Log("触发了事件");
        _loaderUrl.Set("ui://Test/Icon");
        _testList.Set(new List<string> { "a", "b", "c" });

        // _slideValue.Set(70);
        ConsoleUtils.Log(_slideValue.Get(), _input.Get());
    }
}
```