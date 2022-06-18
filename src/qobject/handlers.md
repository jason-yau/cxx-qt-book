<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Handlers

Handler 用于对 Qt 事件循环线程上的事件作出反应。这允许 Rust 对来自 C++ 的事件做出反应，在 Qt 前端线程上处理来自后端 Rust 线​​程的触发，并避免死锁。

下面列出了一些可用的 handler

  * PropertyChangeHandler 在属性值发生更改时进行处理
  * UpdateRequestHandler 用于处理 Qt 事件循环线程上的更新请求，有关详细信息，请参阅[线程](../concepts/threading.md)章节。

## PropertyChangeHandler

当 [Data 结构体](./data_struct.md) 中定义的属性发生改变时，无论是通过 Rust 调用 setter 还是通过 QML/C++ 调用 setter，我们都可以使用 `PropertyChangeHandler`.

下面示例中，监听了 number 属性，并在 `number` 属性变化时触发 `handle_property_change`。它使用一个在 [data 结构体](./data_struct.md)中定义的属性名称自动生成的 `Property` 枚举。

请注意，这是从 Qt 事件循环线程调用的。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/handler_property_change.rs:book_macro_code}}
```

## UpdateRequestHandler

当后端 Rust 线​​程使用 `UpdateRequester` 请求 Qt 线程通过 `request_update` 来进行同步时， 这会触发 `UpdateRequestHandler` 的 `handle_update_request` 方法。

例如，在可调用函数中，[`CppObj`](./cpp_object.md) 用于检索 `UpdateRequester`.

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_cpp_update_requester}}
```

`UpdateRequester` 被移动到线程中，然后在需要时请求更新。

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_request_update}}
```

然后在之后的阶段中从 Qt 事件循环线程触发 `handle_update_request`。它可以遍历 event_queue（例如来自后端线程的通道），以将值更新到 Qt 对象中（通过带有 CppObj 的 process_event）。

注意，这是从 Qt 事件循环线程调用的。

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_update_request_handler}}
```
