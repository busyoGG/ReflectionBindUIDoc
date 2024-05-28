# UI 行为绑定

## 使用方法

在 UI 类中，在对应方法上添加一个特性标签。标签由两个个参数组成：

1. UI 类型。
2. UI 路径。

```C# 
//UI行为为Click，绑定根界面的索引为2的元素
[UIActionBind(UIAction.Click, "2")]
private void OnBtnClick()
{
    ConsoleUtils.Log("点击了按钮");
    EventManager.TriggerEvent("show_console", null);
}
```

## 类型表

|类型 | 枚举 | 辅助特性 |
|:-:|:-:|:-:|
|列表渲染函数|UIAction.ListRender||
|列表单元格类型辅助函数|UIAction.ListProvider||
|列表点击事件监听函数|UIAction.ListClick||
|点击事件|UIAction.Click||
|拖拽开始|UIAction.DragStart|UICondition(bool)：判断是否自体拖拽 |
|拖拽移动过程中|UIAction.DragHold|UICondition(bool)：判断是否自体拖拽 |
|拖拽结束|UIAction.DragEnd|UICondition(bool)：判断是否自体拖拽 |
|放置|UIAction.Drop||
|悬浮|UIAction.Hover||
|滑动条|UIAction.Slider||
|下拉框|UIAction.ComboBox||

## 特殊情况

### 拖拽

拖拽有三种触发情况：

1. 开始拖拽，执行一次。
2. 拖拽过程中，每帧执行。
3. 拖拽结束，执行一次。

拖拽到对应 UI 放置的数据，需要通过内置的方法 `AddDropData` 传递。

```C# 
[UIActionBind(UIAction.DragEnd, "2"),UICondition(false)]
private void OnDrag(EventContext context)
{
    //添加拖拽数据
    AddDropData(1, 2, "测试数据");
    Debug.Log("结束拖拽");
}
```

对于放置的对象，系统内部会自动把拖拽数据传递给目标对象，因此要保证方法接收一个 `object` 参数。

```C# 
[UIActionBind(UIAction.Drop, "3")]
private void OnDrop(object data)
{
    ConsoleUtils.Log("拖拽放置", data);
}
```

每次拖拽开始的时候都会清除上一次的拖拽数据。

### 列表元素拖拽

在列表中，我们有时候也需要进行拖拽操作，这时候打标签的方法就难以实现。

因此在列表中进行拖拽的时候，引入了手动绑定的方法。

`SetDrag` 为绑定拖拽，传入三个参数：

1. 拖拽类型。
2. 目标 UI 组件。
3. 拖拽方法。

`SetDrop` 绑定放置，传入两个参数：

1. 目标 UI 组件。
2. 放置方法。

```C# 
[UIActionBind(UIAction.ListRender, "1")]
private void ItemRenderer(int index, GObject item)
{
    GComponent comp = item as GComponent;
    GTextField content = comp.GetChildAt(0).asTextField;

    content.text = _testList.Get()[index];

    //绑定拖拽
    SetDrag(UIAction.DragStart, comp, () =>
    {
        ConsoleUtils.Log("开始拖拽");
        AddDropData(index);
    });

    //绑定放置
    SetDrop(comp, (object data) => { ConsoleUtils.Log("放置", data); });
}
```

由于在列表拖拽的时候阻止了列表的滚动，因此注意在 Item 之间留出足够的空隙给滚动功能使用或通过其他方法滚动。

### 悬浮窗提示

悬浮窗需要在悬浮事件中调用 `ShowFloatView` 方法。

该方法需要一个悬浮窗的泛型 T，以及 3 个额外参数：

1. name，唯一标识符。
2. UIName，悬浮窗名，用于 FGUI 创建界面。
3. follow，是否跟随鼠标，默认 false。

在鼠标移出 UI 之后，悬浮窗会自动隐藏。

```C# 
[UIActionBind(UIAction.Hover,"6")]
private void ShowHint()
{
    ShowFloatView<HintView>("input_hint","HintView",true);
    ConsoleUtils.Log("显示悬浮窗");
}
```