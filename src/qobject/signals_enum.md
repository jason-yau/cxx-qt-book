<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Signals 枚举

信号枚举定义了 QObject 上应该存在哪些信号。它允许您定义信号名称和信号的参数。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/signals.rs:book_signals_enum}}
```

## 发出信号

要从 Rust 发出信号，请使用 [`CppObj`](./cpp_object.md) 并调用 `emit_queued(Signal)` 或者 `unsafe emit_immediate(Signal)` 方法。

请注意，`emit_immediate` 是不安全的，因为如果 `Q_EMIT` 用 `Qt::DirectConnection`的方式连接到的同一个已经被 Q_EMIT 的 QObject 上的 Rust 可调用函数，则可能导致死锁，因为这会尝试锁定已锁定的 `RustObj`。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/signals.rs:book_rust_obj_impl}}
```
