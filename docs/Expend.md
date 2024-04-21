# 拓展

## UI类型

在项目工程文件夹中的 `Assets\Script\UI` 目录下的 `UIEnum` 文件中，修改 `UIType` 枚举的内容可以实现类型的拓展。

UI 类型同时作用于 UI 组件绑定和数据绑定。

在同级目录的 `UIBase` 文件中可以实现对应类型的功能。

修改 UI 组件绑定，则需要在 `BindComp` 方法中修改内容。

修改 UI 数据绑定，则需要在 `BindData` 方法中修改内容。

## UI行为

在项目工程文件夹中的 `Assets\Script\UI` 目录下的 `UIEnum` 文件中，修改 `UIAction` 枚举的内容可以实现行为的拓展。

在同级目录的 `UIBase` 文件中可以实现对应行为的功能。

通过修改 `BindAction` 的内容即可实现自定义行为具体的实现逻辑。

## UI监听

本类型实现方法唯一，不需要拓展。

如果真的需要拓展，请修改 `BindListener` 方法。