<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 类型

## 基础类型

这些类型可用于可调用函数的属性、参数或返回类型，以及信号中的参数，无需任何转换。

它们在桥的 C++ 和 Rust 端都显示为它们的常规类型。

| Rust 类型 | C++ 类型 |
|-----------|----------|
| bool      | bool     |
| f32       | float    |
| f64       | double   |
| i8        | qint8    |
| i16       | qint16   |
| i32       | qint32   |
| u8        | quint8   |
| u16       | quint16  |
| u32       | quint32  |

TODO：请注意，目前暂不支持 u64/quint64 ( [https://github.com/KDAB/cxx-qt/issues/36](https://github.com/KDAB/cxx-qt/issues/36) )。

## 自定义类型

这些类型是自定义的，在 Rust 和 C++ 之间传递前需要特殊处理，为此，我们在 cxx_qt_lib crate 中提供了一些辅助类型。

在这些自定义类型中，有两种需要考虑

  * 简单的
  * 复杂的

### 自定义简单类型

自定义简单类型，和基础类型一样，可用于可调用函数中的属性、参数或返回类型，以及信号中的参数，无需任何转换。

在 Rust 端，自定义简单类型以 cxx_qt_lib 辅助类型的形式出现。

注意，当它们在可调用函数中用作参数类型时，应该以引用的方式传递，如 `pointf: &QPointF`，当是属性或返回类型时，应该以值传递，如 `QPointF`。

| Rust 类型 | C++ 类型 |
|-----------|----------|
| cxx_qt_lib::QDate | QDate |
| cxx_qt_lib::QPoint | QPoint |
| cxx_qt_lib::QPointF | QPointF |
| cxx_qt_lib::QRect | QRect |
| cxx_qt_lib::QRectF | QRectF |
| cxx_qt_lib::QTime | QTime |

### 自定义复杂类型

自定义复杂类型封装了一个指向 C++ 类型的唯一指针，它们的使用方式与自定义普通类型相同，但 CXX-Qt 自动编写封装来转换该类型的 C++ 唯一指针和该类型的 Rust 封装。

在 Rust 端，自定义复杂类型以 cxx_qt_lib 辅助类型的形式出现。

注意，当它们在可调用函数中用作参数类型时，应该以引用的方式传递，例如 `color: &QColor`，当是属性或返回类型时，应该值以值传递，例如 `QColor`。同样，字符串类型 `&str` 以引用的形式传递，`String` 以值的形式进行传递。

| Rust 类型 | C++ 类型 |
|-----------|----------|
| cxx_qt_lib::QColor | QColor |
| cxx_qt_lib::QDateTime | QDateTime |
| String or &str | QString |
| cxx_qt_lib::QUrl | QUrl |
| cxx_qt_lib::QVariant | QVariant |

下面是一个以 QVariant 作为参数、返回类型和属性的例子。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/types.rs:book_macro_code}}
```

## 未来可能支持的类型

  * Enums
  * Lists
