# UI数据绑定

## 使用方法

在UI类中，在对应方法上添加一个特性标签。标签由一个参数组成：

1. 事件名。

```C# 
[UIListenerBind("show_console")]
private void ShowConsole(ArrayList data)
{
    //具体实现......
}
```