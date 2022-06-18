<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# QQmlExtensionPlugin

Qt 允许在运行时从目录加载包含对象定义的插件，而不是嵌入到应用程序中。

这允许在业务逻辑和 GUI 代码的学科之间进行清晰的划分。

CXX-Qt 可以生成插件和 qmldir 文件，以便您可以将 Rust 对象作为插件加载到您的应用程序中。

使用 QQmlExtensionPlugin 时，您的项目的文件夹结构可能如下所示，您可以看到“core”和“ui”之间的清晰划分

```ignore
src/
 - core/
   - build.rs
   - Cargo.toml
   - CMakeLists.txt
   - src/
     - lib.rs
 - ui/
   - main.qml
   - qml.qrc
 CMakeLists.txt
 main.cpp
```

## Rust build.rs 的更改

在您 `build.rs` 指定要通过调用 `qqmlextensionplugin` 方式来使用 QQmlExtensionPlugin，如下例子中所示。

在这里，您写上 QML 的导入名称和您用于生成的插件目标的名称。

```rust,ignore,noplayground
{{#include ../../../examples/qml_extension_plugin/core/build.rs:book_build_rs}}
```

## CMake 的更改

以下示例显示了用于构建扩展插件的 CMake 定义。
The following example shows the CMake definition for building an extension plugin.

注意，文件夹结构必须与 QML 导入名称匹配，如 `import foo.bar 1.0` 意味着 `foo/bar` 文件夹需要包含插件和 qmldir 文件。

```cmake,ignore
{{#include ../../../examples/qml_extension_plugin/core/CMakeLists.txt:book_cmake_generation}}
```

## Qt C++ 的改变

要在运行时加载插件，请将包含插件的目录添加到 QML 导入路径中。

```cpp,ignore
{{#include ../../../examples/qml_extension_plugin/main.cpp:book_extension_plugin_register}}
```
