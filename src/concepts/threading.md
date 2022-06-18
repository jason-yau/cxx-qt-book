<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 线程

## 概念

线程的一般概念是，当执行 Rust 代码时，在 C++ 端获取了一个锁，以防止从多个线程执行 Rust 代码。

这意味着直接从 C++ 调用的 Rust 代码（例如可调用函数和 handlers）在 Qt 线程上执行。

我们提供了一种解决方案来防止从信号连接进入死锁，例如，如果属性更改信号连接到 C++/QML 端的可调用函数，如果属性更改是从 Rust 可调用对象触发的，则将无法获取锁。解决方案是将可能导致死锁的事件发布到队列中，例如信号触发，然后在 Rust 可调用函数的锁被释放之后，在下一个事件循环发生时执行这些事件。

如果 Rust 代码需要监听属性变化，可以在 [RustObj Handlers](../qobject/handlers.md) 中实现 handlers（例如 PropertyChangeHandler）。这是直接在 Qt 线程的事件循环中调用的。

<div style="background-color: white; padding: 1rem; text-align: center;">

![Threading Abstract](../images/threading_abstract.svg)

</div>

## 多线程

为了在 Rust 端实​​现安全的多线程，我们使用 `UpdateRequester`. Rust 线​​程在哪里启动（例如一个可调用函数）`UpdateRequester` 就应该被克隆到该线程中。

然后，当后端线程需要更新 Qt 对象中的值时，它会请求更新，这将被发布到与上面相同的队列中。一旦事件循环发生，就会在 [RustObj handlers](../qobject/handlers.md) 中调用 `UpdateRequestHandler`，这样您就可以安全地调用 setter 或从 Qt 线程发出信号并将您的状态同步到前端。

我们建议使用线程中的通道来发送信号枚举，或者发送在之后处理的 `UpdateRequestHandler` 的值.

下面是一个完整的 Rust 多线程对象示例。

```rust,ignore,noplayground
{{#include ../../../examples/qml_with_threaded_logic/src/lib.rs:book_macro_code}}
```
