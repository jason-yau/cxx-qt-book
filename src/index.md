<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# CXX

这库为 Qt 代码和 Rust 代码之间的融合提供了一种安全的机制，这不同于常规的 Rust Qt 绑定。

我们承认 Qt 代码和 Rust 代码有不同的风格，因此不能直接进行封装。

我们使用 [CXX](https://cxx.rs/) 来[桥接](./concepts/bridge.md)，而不是一对一的绑定，这样可以让我们编写常规的 Qt/Rust 代码。

我们觉得这比常规的绑定更强大，因为这在 Qt 和 Rust 之间提供了安全的 API 和安全的[多线程](./concepts/threading.md)。

为了使 Qt 和 Rust 代码进行融合，我们为 Rust 提供了常见的 [Qt 类型](./concepts/types.md)，并提供了让我们跨越桥梁使用常用的 [Qt 风格](./concepts/qt.md)的方法。

如下图所示，通过使用宏和代码生成，开发者写了一个带有 CXX-Qt 宏标记的 `QObject`。然后 CXX-Qt 生成 C++ 对象，并使用宏展开来定义 [CXX](https://cxx.rs/) 桥，以达到 C++ 和 Rust 之间交互的目的。

<div style="background-color: white; padding: 1rem; text-align: center;">

![Overview of CXX-Qt concept](./images/overview_abstract.svg)

</div>

如果您是 CXX-Qt 的新手，建议访问我们的[入门指南](./getting-started/index.md)。

要获取有关 CXX-Qt 中的 QObject 有哪些特性，请参阅 [QObject 章节](./qobject/index.md)。如果您有兴趣深入了解 CXX-Qt 的相关概念，请参阅[概念章节](./concepts/index.md)，其中详细解释了 CXX-Qt 引入的一些概念。

请注意，我们仅支持 64 位 x86 Linux 的操作系统，但我们计划在未来添加 arm 64 位操作系统以及 macOS 和 Windows 操作系统的支持。(译注：亲测在 Windows、MacOS、Linux 上均可以使用，估计是文档还没有及时更新)
