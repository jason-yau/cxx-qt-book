<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Qt

## 可调用函数

可以使用 [RustObj 结构体](../qobject/rustobj_struct.md)定义可调用函数，这些导出为带有 `Q_INVOKABLE` 的 C++ 类上的方法，以便在 QML 访问。
Invokables can be defined using the [RustObj Struct](../qobject/rustobj_struct.md), these will be exposed as methods on the C++ class with `Q_INVOKABLE` so that they are accessible for QML too.

## 属性

可以使用 [Data 结构体](../qobject/data_struct.md)定义属性，这会生成 getter 和 setter 方法、变化信号和 C++ 类上的 `Q_PROPERTY`，因此也作为 QML 属性。

## 信号

可以使用 [Signals 枚举](../qobject/signals_enum.md)来定义信号，这些将导出为 C++ 类中的 `Q_SIGNALS`，因此也会导出给 QML。

## 变化事件

您可以通过 RustObj Struct 中可用的 [handlers](../qobject/handlers.md) 来监听属性的变化。这些 handlers 从 Qt 事件循环线程调用以保持[线程安全](./threading.md)。
