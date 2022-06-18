<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Cpp 对象

要访问和改变 C++ 端，例如属性，我们需要一个句柄来访问 C++ 对象。为了安全地做到这一点，CXX-Qt 提供了一种 `CppObj` 类型，它是生成的 C++ 对象的安全封装。
To access and mutate the C++ side, eg properties, we need a handle to access the C++ object. To do this safely CXX-Qt provides a `CppObj` type which is a safe wrapper around the generated C++ object.

## 可调用函数

要使用 `CppObj`，请在可调用的参数中添加 `cpp: &mut CppObj`。

如果 [`Data` 结构体](./data_struct.md)有一个名为 `number: i32` 的字段，那么您可以使用 `CppObj` 的 `number(&self) -> i32` 和 `set_number(&mut self, number: i32)` 来访问属性。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/rust_obj_invokables.rs:book_cpp_obj}}
```

如果有一个 [`Signals` 枚举](./signals_enum.md)，那么你可以调用 `CppObj` 的 `emit_queued(&mut self, Signals)` 或 `unsafe emit_immediate(&mut self, Signals)` 发出一个信号。

注意，`emit_immediate` 是不安全的，因为如果 `Q_EMIT` 用 `Qt::DirectConnection` 方式连接到同一个已经被 `Q_EMIT` 的 QObject 上的 Rust 可调用函数，则可能导致死锁，因为这尝试锁定已锁定的 `RustObj`。

```rust,ignore,noplayground
impl RustObj {
    #[invokable]
    fn invokable(&self, cpp: &mut CppObj) {
        unsafe { cpp.emit_immediate(Signal::Ready); }

        cpp.emit_queued(Signal::DataChanged { data: 1 });
    }
}
```

## 线程

`CppObj` 可以通过 `update_requester(&self) -> cxx_qt_lib::update_requester::UpdateRequester` 方法来用于线程上访问 `UpdateRequester`。

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_cpp_update_requester}}
```

`UpdateRequester` 被移动到 Rust 线​​程中，然后在 `request_update(&self) -> bool` 被调用时触发Qt 线程上的 [`UpdateRequestHandler`](./handlers.md) 。

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_request_update}}
```

## 序列化和反序列化

如 [Data struct](./data_struct.md) 的（反）序列化部分所述，`CppObj` 有一个方法 `grab_values_from_data`，它可以从 `Data` 加载值到 C++ 实例中。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/serialisation.rs:book_grab_values}}
```

## 类型封装

当使用 getter 或 setter 访问 C++ 属性值时，Rust getter 和 setter 会自动执行 [C++ 和 Rust 类型](../concepts/types.md) 之间的任何转换。这允许 Rust 代码使用 Rust 类型表示，而无需转换为 C++ 类型或从 C++ 类型转换。

TODO：解释我们在之后如何从子对象等中将其用于 borrowRustObj（并注意此处的线程），例如 nested_object() 可以返回 `Borrow<T>`。

TODO：一旦我们有了 borrow_rust_obj()，请解释如何使用它来访问另一个对象 RustObj ( [https://github.com/KDAB/cxx-qt/issues/30](https://github.com/KDAB/cxx-qt/issues/30) )。
