<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Summary

[介绍](./index.md)

---

- [入门](./getting-started/index.md)
    - [Rust 中的 QObjects](./getting-started/1-qobjects-in-rust.md)
    - [我们第一个 CXX-Qt 模块](./getting-started/2-our-first-cxx-qt-module.md)
    - [导出到 QML](./getting-started/3-exposing-to-qml.md)
    - [创建 QML GUI](./getting-started/4-qml-gui.md)
    - [用 CMake 构建](./getting-started/5-cmake-integration.md)
- [QObject](./qobject/index.md)
    - [宏](./qobject/macro.md)
    - [Data 结构体](./qobject/data_struct.md)
    - [RustObj 结构体](./qobject/rustobj_struct.md)
    - [Cpp 对象](./qobject/cpp_object.md)
    - [Signals 枚举](./qobject/signals_enum.md)
    - [Handlers](./qobject/handlers.md)
- [概念](./concepts/index.md)
    - [Bridge](./concepts/bridge.md)
    - [Qt](./concepts/qt.md)
    - [类型](./concepts/types.md)
    - [Build.rs 和 Cargo.toml](./concepts/build_rs_and_cargo.md)
    - [CMake 集成](./concepts/cmake.md)
    - [注册类型](./concepts/register_types.md)
    - [QQmlExtensionPlugin](./concepts/qqmlextensionplugin.md)
    - [线程](./concepts/threading.md)
    - [嵌套对象](./concepts/nested_objects.md)
- [内部剖析](./internal/index.md)
    - [构建](./internal/build.md)
