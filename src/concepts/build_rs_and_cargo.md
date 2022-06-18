<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# Build.rs

我们需要指定一个 build.rs 文件以便我们可以解析宏并生成相关的 C++ 代码。

可以使用以下选项

* 指示应解析哪些文件以查找宏
* 启用构建为 [QQmlExtensionPlugin](./qqmlextensionplugin.md)
* 确定生成的 C++ 代码的 clang 格式样式
* 为生成的 Rust 类型指定自定义 C++ 命名空间

build.rs 脚本如下所示

```rust,ignore,noplayground
{{#include ../../../examples/qml_minimal/build.rs:book_build_rs}}
```

如果您要注册为插件，build.rs 如下所示

```rust,ignore,noplayground
{{#include ../../../examples/qml_extension_plugin/core/build.rs:book_build_rs}}
```

非默认 C++ 命名空间可能如下所示

请注意，命名空间是一个列表，所以 `vec!["a", "b", "c"]` 会变成 `a::b::c`

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/build.rs:book_build_rs}}
```

# Cargo.toml

`Cargo.toml` 项目文件只需很少的更改即可使用 CXX-Qt。

首先，我们目前需要构建为静态库（因为 Rust 库静态链接到 C++ 可执行文件或库）。

```cargo
{{#include ../../../examples/qml_minimal/Cargo.toml:book_static_lib}}
```

在项目中使用 CXX-Qt 需要以下依赖项。

```cargo
{{#include ../../../examples/qml_minimal/Cargo.toml:book_dependencies}}
```

最后，build.rs 文件需要以下构建依赖项才能运行。

```cargo
{{#include ../../../examples/qml_minimal/Cargo.toml:book_build_dependencies}}
```

注意，如果您使用的是 [crates.io](https://crates.io/)，则对于依赖项，则不需要 path 参数，而是像往常一样写上版本号即可（例如 `cxx-qt = "0.3"`）。
