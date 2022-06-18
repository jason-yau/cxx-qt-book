<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Data 结构休

Data 结构体定义了 QObject 上应该存在哪些属性。它还允许您通过实现 `Default` trait 来为属性提供初始值。

注意，您还可以在 Data 结构体上使用 serde 并派生 `Deserialize` 和 `Serialize`，这允许您反序列化和序列化 QObject 中的属性。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/data_struct_properties.rs:book_macro_code}}
```

## Default

如果你想为 QObject 提供默认值，那么不要给 `Data` 结构体派生实现 `Default` trait。

## 属性枚举

调用 `Property` 的枚举是从 `Data` 结构中的字段名称自动生成的，然后可以在 [`PropertyChangeHandler`](./handlers.md)使用。

## 序列化和反序列化

使用 [Serde](https://serde.rs/) 可以（反）序列化 Data 结构体，方法是像往常一样添加派生属性。

要将对象从 `Data` 结构体序列化为字符串，请在 `Data` 结构体上按照往常那样使用 serde，想从可调用函数中获取 `Data` 结构体实例，请使用 `CppObj`，例如 `Data::from(cpp)`，如下面可调用函数 `as_json_str` 中所示.

要将一个对象从字符串反序列化为 `Data` 结构体，请照常使用 serde。这样做的两个主要目的是为 `Data` 实现 `Default` 或使用使用 `CppObj` 的 `grab_values_from_data` 方法，如在 `grab_values` 函数中所示。

注意，Qt 类型还不能（反）序列化( [https://github.com/KDAB/cxx-qt/issues/16](https://github.com/KDAB/cxx-qt/issues/16) )。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/serialisation.rs:book_macro_code}}
```
