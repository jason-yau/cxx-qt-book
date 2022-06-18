<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Leon Matthes <leon.matthes@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# CXX-Qt - 入门

与其他 Qt-Rust 绑定相比，CXX-Qt 的目标不是简单地将 Qt 函数暴露给 Rust，而是将 Rust 完全融合到 Qt 生态系统中。

在本指南中，我们将介绍一个虽小但完整的示例，该示例使用 CXX-Qt 在 Rust 中创建您自己的 QObject，并与小型的 QML GUI 集成。由于 CXX-Qt 旨在将 Rust 集成到现有的 Qt 生态系统中，因此在阅读本指南前，您应该具备 Qt 和 QML 的基础知识。如果您还不熟悉 Qt/QML，请分别查看 [Qt Getting started guide](https://doc.qt.io/qt-5/gettingstarted.html) 或 [QML intro](https://doc.qt.io/qt-5/qmlapplications.html)。

在本入门指南中，我们将首先了解 CXX-Qt 如何与 Qt 的对象系统集成并且在 [Rust 中定义 QObject](./1-qobjects-in-rust.md) 的。然后我们直接通过实践，并将我们第一个 [QObject 定义为 Rust 模块](./2-our-first-cxx-qt-module.md)。当我们完成后，将[定义好的 QObject 暴露给 QML](./3-exposing-to-qml.md)。然后[使用 QML 编写我们的 GUI](./4-qml-gui.md)。最后，[通过 CMake 将我们的代码融合](./5-cmake-integration.md)，以便我们可以构建并运行。

注意，我们仅支持 64 位 x86 Linux 的操作系统，但我们计划在未来添加 arm 64 位操作系统以及 macOS 和 Windows 操作系统的支持。(译注：亲测在 Windows、MacOS、Linux 上均可以使用，估计是文档还没有及时更新)

废话少说，我们[开始](./1-qobjects-in-rust.md)吧
