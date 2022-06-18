<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Leon Matthes <leon.matthes@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 我们的第一个 CXX-Qt 模块

与所有 Rust 一样，我们首先要创建一个 cargo 项目。

```bash
cargo new --lib qml-minimal
```

注意此处的 `--lib` 选项。我们在 Rust 中创建的是一个静态库，而不是一个可执行文件。当我们[将 Rust 项目与 CMake 集成](./5-cmake-integration.md)时，再讨论这方面的细节。

如上一节所述，要定义一个新的 QObject 子类，我们需要创建一个 Rust 模块。所以让我们进入 `src/lib.rs` 文件。我们将修改这个文件，直到它看起来像这样：

```rust,ignore
{{#include ../../../examples/qml_minimal/src/lib.rs:book_cxx_qt_module}}
```

有很多东西东西要写，所以让我们一步一步来。从模块定义开始：

```rust,ignore
{{#include ../../../examples/qml_minimal/src/lib.rs:book_make_qobject_macro}}
```

因为我们将 `#[make_qobject]` 宏添加到模块定义中，CXX-Qt 会从这个模块创建一个新的 QObject 子类。在我们的例子中，新的 QObject 子类将命名为 `MyObject`，因为 CXX-Qt 会自动将 Rust 的 snake_case 转换为 Qt 默认的 PascalCase。CXX-Qt 使用 Rust 和 C++ 的代码风格，因此它会尽最大的努力保持 C++ 和 Rust 的样式一致。

为了使 `#[make_qobject]` 宏起作用，我们首先需要定义将存在于新 C++ 对象中的数据。这是通过 `Data` 结构体来完成的：

```rust,ignore
{{#include ../../../examples/qml_minimal/src/lib.rs:book_data_struct}}
```

这意味着新创建的 QObject 子类将有两个属性作为成员：`number` 和 `string`. 对于包含多个单词的名称，例如 `my_number`，CXX-Qt 将再次执行 snake_case 到 camelCase 的转换，以符合 C++/QML 命名约定。

注意，我们在这里使用的数据类型是普通的 Rust 数据类型。CXX-Qt 会自动将这些类型转换为它们的 C++/Qt 等价类型。在我们的例子中，这意味着：

- `number: i32` -> `int number`
- `string: String` -> `QString string`\
有关更多可以类型的详细信息，请参阅[Qt 类型页面](../concepts/types.md)。

你可能也注意到了这里的 `#[derive(Default)]`。目前 Data 结构需要始终是默认可构造的。`Default` 的实现返回的数据将被转换为适当的 C++ 类型并分配给任何新构造的 `MyObject` 实例的属性。或者，我们也可以为 Data 提供自己的 `Default` 实现。

现在我们已经定义了将存在于 C++ 端的数据，让我们来看看 Rust 端：

```rust,ignore
{{#include ../../../examples/qml_minimal/src/lib.rs:book_rustobj_struct}}
```

在我们的例子中，这只是一个空结构体。但是，`RustObj` 可以包含任何我们想要的数据。它不会转换为 C++ 类，因此它不像 `Data` 结构体那样只允许在结构体中使用与 Qt 兼容的数据类型。

这里要注意的重要一点是 `RustObj`, 像 `Data` 结构体那样必须实现 `Default` trait。`MyObject` 类的每个实例都会使用 `Default` trait 来自动创建一个对应 `RustObj` 的实例。

仅仅因为 `RustObj` 结构体不包含任何数据，这并不意味着它不是我们 `MyObject` 类的重要组成部分。这是因为实际上通过它的 `impl` 来定义了我们类的行为：

```rust,ignore
{{#include ../../../examples/qml_minimal/src/lib.rs:book_rustobj_impl}}
```

在我们的例子中，我们定义了两个新函数：

- `increment_number`
  - 增加 `MyObject` 的 number 的值。
  - 由于 number 位于 C++ 端，它使用由 CXX-Qt 生成的 `CppObj` 封装，并为每个属性提供适当的 setter 和 getter 函数。
  - 该名称在 C++ 中会转换为 `incrementNumber`。
- `say_hello`
  - 打印传递过来的 number 和 string。
  - 该名称在 C++ 中会转换为 `sayHello`。

这两个函数都标有 `#[invokable]` 宏，这意味着这些函数将被添加到 C++ `MyObject` 代码中，并且也可以在 QML 中调用。

除了用 `#[invokable]` 宏标记的函数之外，`RustObj` impl 只是一个普通的 Rust 结构体 impl，可以包含普通的 Rust 函数，也可以照常调用 invokable 函数。

就这样。我们已经在 Rust 中定义了我们的第一个 QObject 子类。不难吧？

现在开始在 [Qt 中使用它](./3-exposing-to-qml.md)吧。
