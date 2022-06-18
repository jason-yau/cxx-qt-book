<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Bridge

CXX-Qt 使用 [CXX](https://cxx.rs/) 以一种安全的方式在 C++ 和 Rust 之间架起桥梁。

CXX-Qt 提供了用于声明 Qt 对象（例如 [QObject](../qobject/index.md)）的宏，而且仍然保持着 Rust 代码风格。

我们提供一些 [Qt 类型](./types.md)来辅助在 Rust 和 Qt 之间传递通用数据类型。

当 Rust 函数暴露给 C++ 时，我们会自动执行 snake_case 和 camelCase 之间的转换。因此项目（例如属性和可调用函数）在 C++ 中显示为  camelCase 风格，而在 Rust 中显示为  snake_case 风格。

注意，Rust [`RustObj`](../qobject/rustobj_struct.md)构造的 Qt 对象由 C++ 端持有。因此，当 C++ 对象销毁时，Rust 对象也会销毁。在将来，会有用于从 C++ 对象的构造函数或者析构函数中执行 Rust 代码的 [handlers](../qobject/handlers.md) [https://github.com/KDAB/cxx-qt/issues/13](https://github.com/KDAB/cxx-qt/issues/13)。
