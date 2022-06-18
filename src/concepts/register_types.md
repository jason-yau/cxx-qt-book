<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# C++ 注册 QML 类型

注册生成的 QML 类型有两个选项，作为 [QQmlExtensionPlugin](./qqmlextensionplugin.md)使用或将类型注册到引擎。

## 注册到引擎

如果要将类型注册到引擎，首先包含生成的对象（由 Rust 模块的名称确定）。

```cpp,ignore
{{#include ../../../examples/qml_minimal/src/main.cpp:book_cpp_include}}
```

然后以正常方式注册 QML 类型。

```cpp,ignore
{{#include ../../../examples/qml_minimal/src/main.cpp:book_qml_register}}
```

注意，将来可能会有一个 helper 调用，即使不使用插件也可以注册所有类型( [https://github.com/KDAB/cxx-qt/issues/33](https://github.com/KDAB/cxx-qt/issues/33) )。

## 使用 QQmlExtensionPlugin

如果您使用的是 QQmlExtensionPlugin，请确保生成的库位于导入路径中。

```cpp,ignore
{{#include ../../../examples/qml_extension_plugin/main.cpp:book_extension_plugin_register}}
```

## QML

一旦您使用了上述任何一种方法将类型注册到引擎，那么您可以从 QML 中像普通的 C++ 模块一样包含这些类型。

```qml,ignore
{{#include ../../../examples/qml_minimal/src/main.qml:book_qml_import}}
```
