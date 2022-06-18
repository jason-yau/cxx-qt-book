<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Leon Matthes <leon.matthes@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 暴露我们的 QObject 子类给 QML

在[定义了我们的第一个 CXX-Qt 模块](./2-our-first-cxx-qt-module.md)之后，我们准备创建 Qt 应用程序并将新 `MyObject` 类导出给 QML。

最简单的方法是在 `src` 文件夹中旁边的 `lib.rs` 文件添加一个 `main.cpp` 文件。

```cpp,ignore
{{#include ../../../examples/qml_minimal/src/main.cpp:book_main_cpp}}
```

这个 C++ 文件创建一个基本的 Qt 应用程序并执行它。如果您对此不熟悉，我建议您查看[Qt documentation](https://doc.qt.io/qt-5/gettingstarted.html)。

与普通的 Qt 应用程序相比，有两个显着的变化：

```cpp,ignore
{{#include ../../../examples/qml_minimal/src/main.cpp:book_cpp_include}}
```

```cpp,ignore
{{#include ../../../examples/qml_minimal/src/main.cpp:book_qml_register}}
```

对于我们在 Rust 中定义的每个 QObject 子类，CXX-Qt 都会生成一个对应的 C++ 类。此类包含在第一个代码片段中。它们将始终位于 `cxx-qt-gen/include/` 包含路径中并使用 snake_case 命名约定。

然后第二个代码片段将该类导出到 QML。就 Qt 而言，`MyObject` 与任何其他 QObject 子类的工作方式相同，这正是它所关注的。这里唯一需要注意的是，类是在 `cxx_qt::my_object` 命名空间中生成的。`my_object` 是我们之前定义 Rust 模块的名称。

由于我们希望在[Qt 资源系统](https://doc.qt.io/qt-5/resources.html)中包含我们的QML GUI `main.qml` 文件，因此我们还必须在 `src` 文件夹中添加一个 `qml.qrc` 文件：

```qrc,ignore
{{#include ../../../examples/qml_minimal/src/qml.qrc:book_rcc_block}}
```

就是这样。我们现在可以[使用来自 QML 的酷炫的新类](./4-qml-gui.md)了。
