<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 宏

我们定义一个模块（它成为我们的 Qt 对象名称），然后添加 `make_qobject` 宏。

下面的示例是将模块的内容导出 `DataStructProperties` 给 Qt/QML。

请注意，对象名称需要是唯一的，以避免冲突，将来可能会使用完整的模块路径来帮助避免冲突[https://github.com/KDAB/cxx-qt/issues/19](https://github.com/KDAB/cxx-qt/issues/19) - 但这并不能阻止试图注册两个具有相同名称的 QML 类型。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/data_struct_properties.rs:book_macro_code}}
```

注意：这可能会在未来发生变化，以允许在导出到 QML 时定义基类或选项，并且可能会成为命名空间 `#[cxx_qt(QObject)]`( [https://github.com/KDAB/cxx-qt/issues/22](https://github.com/KDAB/cxx-qt/issues/22) )。
