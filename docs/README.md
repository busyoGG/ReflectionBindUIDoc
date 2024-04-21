# ReflectionBindUI文档

各部分功能说明及使用方式文档

## 文档说明

* 本文档使用[docsify](https://docsify.js.org/#/zh-cn/)构建。
* 项目工程见[ReflectionBindUI](https://github.com/busyoGG/ReflectionBindUI)。
* 本项目使用FGUI，如需使用UGUI或其他，请自行调整UI获取的方法以及相关UI行为的方法（如拖拽的实现）。
* 脚本需要继承 `BaseView`（带手动调用的生命周期）或 `UIBase`（没有生命周期）。
* 项目原理说明见[Unity-利用特性反射来绑定UI](https://busyo.buzz/article/999bc53b642c/)。

<!-- * `_sidebar.md` 文件为侧边栏导航配置文件。 -->

<!-- * 如果需要增加侧边栏项目，请在docs文件夹下新建md文档，并且配置到侧边栏导航文件中。 -->

<!-- * `AfterProgress.js` 为页面处理代码，一些页面的布局处理效果在这里实现。 -->

* `index.html` 中的 `plugins` 的 `function(hook)` 提供自定义插件的修改能力，详情见[开发插件](https://docsify.js.org/#/zh-cn/write-a-plugin)

## 特殊插件使用说明

### tab

<!-- tabs:start -->

#### **English**

Hello!

#### **French**

Bonjour!

#### **Italian**

Ciao!

<!-- tabs:end -->

按照如下格式书写

```markdown
<!-- tabs:start -->

#### **English**

Hello!

#### **French**

Bonjour!

#### **Italian**

Ciao!

<!-- tabs:end -->
```