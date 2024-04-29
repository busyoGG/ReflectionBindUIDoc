# 快速开始

## 简介

利用特性 Attribute 来反射初始化 UI 组件，使其和数据进行双向绑定，更新数据的时候同步更新 UI，更新 UI 的时候同步更新数据（只更新当前界面定义的数据，原始数据需要手动更新）。

在此基础上拓展了 UI 动作绑定和UI监听绑定，这样只需要对方法标记特性，就可以自动完成对应的绑定工作。

基本上每一个功能和属性都只专注于其自身，只有在需要手动获取某个 UI 元素并且进行操作的时候会在实现代码中产生交叉。

## 基本用法

### 使用方式

基本用法请见导航栏 UI 绑定部分内容。

所有绑定的数据都必须是 `StringUIProp`、`DoubleUIProp`、`UIListProp` 其中一种。

其中 `UIListProp` 保存的数据是 `List<T>` 类型，数据更新的时候只更新 UI 列表元素的渲染数量，即 `numItems`。渲染函数对应索引的数据请自行获取。

### UI获取

UI 元素路径，即 path ，可以通过两种方式设置。

1. 索引数字。
2. UI 名字。

虽然有两种获取 UI 的方式，但是传入的数据都是 `string` 类型，索引也不例外。

### 更新数据

本项目的数据和 UI 是双向绑定的。

只需要使用 Set 方法设置数据即可，UI 自己会更新。

```C#
//设置了装载器的图片路径，装载器组件会自动设置src="ui://Test/Icon"
_loaderUrl.Set("ui://Test/Icon");
```

如果 UI 更新了数据，则通过 Get 方法获取的时候会自动更新。

```C#
//获取参数的值
_loaderUrl.Get();
```

> 注意：UI监听使用 [事件管理器](https://github.com/busyoGG/EventManagerForUnity) 项目的事件系统，如果需要使用自己的事件监听方法请参考导航栏中的拓展选项。

### UI 管理器

通过 UI 管理器，我们可以很轻松地创建和隐藏 UI。

#### 展示 UI

使用 `UIManager.Ins().ShowUI<T>(string folder, string package, string name, UINode parent = null)` 来展示 UI。

管理器当前 UI 路径在 `Resources` 文件夹内，因此 UI 加载以该路径为主。

* folder: UI 所在文件夹。
* package: 需要展示的 UI 的 FGUI 包名。
* name: 自定义名称。
* parent: UI 树层级。

假如 UI 在 `Resources\FGUI` 文件夹下，则：

```C# 
UIManager.Ins().ShowUI<TestView>("FGUI", "Test", "TestView0");
```

如果我们需要在某窗口打开一个子级窗口，则使用如下方法：

```C# 
UINode first = UIManager.Ins().ShowUI<TestView>("FGUI", "Test", "TestView0");
UIManager.Ins().ShowUI<TestView>("FGUI", "Test", "TestView1", first);
```

#### 隐藏 UI

使用 `UIManager.Ins().HideUI(UINode ui)` 来隐藏窗口。

当窗口隐藏的时候，所有子级窗口都会一并隐藏。

```C# 
//uiChild是BaseView保存的UINode
HideUI(uiChild);
```

#### 获取 UI

使用 `UIManager.Ins().GetUI(string name, UINode parent = null)` 来根据名称获取 UI 窗口。

默认不传入 `parent` 的话会从根节点开始查找；传入的话从 `parent` 节点开始查找。

```C# 
UINode node = UIManager.Ins().GetUI("TestView0");
```

#### 销毁 UI

使用 `UIManager.Ins().DisposeUI(UINode ui);` 来销毁 UI。

当窗口销毁的时候，所有子级窗口都会一并销毁。

```C# 
UIManager.Ins().DisposeUI(uiNode);
```

## 案例

不包含所有调用，但是大概的调用都有。

```C# 
using FairyGUI;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[UIClassBind(UIClass.Drag,"Retop","n10"),UIColor(1,1,1,0.5f)]
public class TestView : BaseView
{
    [UIDataBind(UIType.TextField, "1")]
    private StringUIProp _testText { get; set; }

    [UIDataBind(UIType.List, "n1")]
    private UIListProp<string> _testList { get; set; }

    [UIDataBind(UIType.Loader, "4")]
    private StringUIProp _loaderUrl { get; set; }

    [UIDataBind(UIType.Slider, "5")]
    private DoubleUIProp _slideValue { get; set; }

    [UIDataBind(UIType.TextInput, "6")]
    private StringUIProp _input { get; set; }
    
    [UIDataBind(UIType.ComboBox,"9","1","b")]
    private DoubleUIProp _comboBoxIndex { get; set; }

    [UICompBind(UIType.Loader, "4")]
    private GLoader _loader { get; set; }


    [UIActionBind(UIAction.ListRender, "2")]
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

    [UIActionBind(UIAction.Click, "3")]
    private void OnBtnClick()
    {
        ConsoleUtils.Log("点击了按钮");
        _comboBoxIndex.Set(1);
        EventManager.TriggerEvent("show_console", null);
    }

    [UIActionBind(UIAction.DragEnd, "3", "Self")]
    private void OnDrag(EventContext context)
    {
        //添加拖拽数据
        AddDropData(1, 2, "测试数据");
        Debug.Log("结束拖拽");
        Debug.Log(GRoot.inst.touchTarget);
    }

    [UIActionBind(UIAction.Drop, "4")]
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

    [UIActionBind(UIAction.Hover,"7")]
    private void ShowHint()
    {
        ShowFloatView<HintView>("input_hint","HintView",true);
        // ConsoleUtils.Log("显示悬浮窗");
    }

    [UIActionBind(UIAction.Slider,"5")]
    private void OnSliderChanged()
    {
        ConsoleUtils.Log("滑动条",_slideValue.Get());
    }
    
    [UIActionBind(UIAction.ComboBox,"9")]
    private void OnComboBoxChanged()
    {
        ConsoleUtils.Log("下拉框",_comboBoxIndex.Get());
    }

    [UIActionBind(UIAction.Click,"n11")]
    private void Close()
    {
        UIManager.Ins().HideUI(uiNode);
        // UIManager.Ins().DisposeUI(uiNode);
    }


    // protected override void TweenIn()
    // {
    //     main.x = -500;
    //     AddTween(TweenTarget.X,500,2000,TweenEaseType.CircOut);
    //     
    //     main.y = 500;
    //     AddTween(TweenTarget.Y,-500,2000,TweenEaseType.Linear);
    //     
    //     main.scaleX = 0.5f;
    //     AddTween(TweenTarget.ScaleX,0.5f,2000,TweenEaseType.CircOut);
    //     
    //     AddTween(TweenTarget.Rotation,50,2000,TweenEaseType.CircOut);
    // }
    //
    // protected override void LateOnShow()
    // {
    //     ConsoleUtils.Log("缓动回调",Time.time);
    // }

    // protected override void TweenOut()
    // {
    //     AddTween(TweenTarget.X,500,2000,TweenEaseType.CircOut);
    // }
}
```