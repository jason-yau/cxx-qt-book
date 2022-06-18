<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Leon Matthes <leon.matthes@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 创建我们的 QML GUI

正如 [Rust 中的 QObject](./1-qobjects-in-rust.md) 一章所述，我们总是希望使用“用正确的工具来做正确的事”。对于 Qt 中的小型现代 GUI，这绝对意味着使用 QML。它具有强大、灵活、声明性的特点，允许我们快速迭代。

因此，让我们在 `src` 文件夹中的其他两个文件旁边添加一个 `main.qml` 文件：

```qml,ignore
{{#include ../../../examples/qml_minimal/src/main.qml:book_main_qml}}
```

如果您不熟悉 QML，我建议您查看[Qt QML intro](https://doc.qt.io/qt-5/qmlapplications.html)。

这段代码将创建一个非常简单的 GUI，它由两个 Label 和两个 Button 组成。这里重要的部分是 `MyObject` 类型的使用。如您所见，我们之前定义的类现在可以在 QML 中使用。

由于它只是另一个 QObject 子类，因此可以在 Qt 属性绑定系统中使用，就像 `myObject.string` 和 `myObject.number`.

Label 简单地显示 `MyObject` 类中定义的数据。我们可以使用这两个 Button 与 `MyObject` 实例进行交互。正如您在此处看到的，CXX-Qt 已将函数名称的 snake_case 转换为 camelCase - `incrementNumber` 和 `sayHi`. 这样 `MyObject`在 QML 中似乎不显示警告了。

这里再次强调一点很重要的，`MyObject` 它只是另一个 QObject 子类，可以像任何其他 `QObject` 子类一样使用。唯一的区别是定义的任何可调用函数都是在 Rust 中定义的，而不是在 C++ 中定义的。对于 QML 来说，这并没有什么区别。

OK，让我们开始[构建和运行](./5-cmake-integration.md)这个项目吧。
