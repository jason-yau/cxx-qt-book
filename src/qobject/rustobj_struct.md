<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# RustObj 结构体

RustObj 结构允许您定义以下部件

  * 暴露给 Qt 的可调用函数
  * 在 RustObj 中使用的私有方法和字段（例如，这对于存储[线程](../concepts/threading.md)的通道很有用）
  * 使用 [`CppObj`](./cpp_object.md) 改变 C++ 的状态
  * 为属性或更新请求实现 [handlers](./handlers.md)

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/rust_obj_invokables.rs:book_macro_code}}
```

## 可调用函数

要定义暴露给 QML 和 C++ 的方法，请在 `RustObj` 结构上添加函数并在函数添加属性 `#[invokable]`。然后将参数和返回类型匹配到 Qt 端。此外，CXX-Qt 会自动在您的可调用函数进行封装，以自动执行 [C++ 和 Rust 类型](../concepts/types.md)之间的任何转换。

注意，要访问 C++ 对象的属性，请使用 [Cpp Object](./cpp_object.md)。

## 私有方法和字段

不同于在 [Data 结构体](./data_struct.md)上定义的字段，`RustObj` 定义的字段不会作为属性公开给 Qt。这可以被认为是“私有的 Rust”字段，并且对于存储线程的通道或 QObject 的内部信息很有用。

`RustObj` 在没有 `#[invokable]` 属性的方法不会暴露给 C++，并且被认为是“私有的 Rust”方法。与字段类似，这些对于线程和内部信息很有用。
