<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Leon Matthes <leon.matthes@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 使用 CMake 构建

```diff,ignore
- 免责声明：CXX-Qt 的 CMake 集成仍处于开发中。
- 目前的状态离最佳状态还很远，未来可能会有很多改善
- 所以不要因为这一章的内容而气馁。
- 欢迎奉献。
```

在开始使用 CMake 构建 Qt 之前，我们首先需要为 Cargo 构建做好准备。如果您使用 `cargo new --lib` 命令生成了项目，您 `Cargo.toml` 文件可能看起来像这样：

```toml,ignore
[package]
name = "qml-minimal"
version = "0.1.0"
edition = "2021"

[dependencies]
```

我们逐步来：

- 指示 cargo 创建一个具有定义名称（“rust”）的静态库，供 CMake 链接。
- 添加 `cxx`, `cxx-qt`, 以及 `cxx-qt-lib` 依赖项。
- 添加 `clang-format` 和 `cxx-qt-build` 作为构建依赖项。

最后，您的 `Cargo.toml` 应该看起来与此类似（请注意，path不需要依赖项）：

```toml,ignore
{{#include ../../../examples/qml_minimal/Cargo.toml:book_all}}
```

然后，我们还需要在 `Cargo.toml` 旁边添加一个名为 `build.rs` 文件：

```rust,ignore
{{#include ../../../examples/qml_minimal/build.rs:book_build_rs}}
```

这就是是在编译时为我们 `MyObject` 类生成的 C++ 代码。它将输出我们之前在 `main.cpp` 中包含的 `cxx-qt-gen/include/my_object.h` 文件。

请注意，所有使用 `#[make_qobject]` 宏的 Rust 源文件都需要包含在此脚本中！在我们的例子中，这需要包含 `src/lib.rs` 文件。

然后我们编写 `CMakeLists.txt` 文件：

```cmake,ignore
{{#include ../../../examples/qml_minimal/CMakeLists.txt:book_tutorial_cmake_full}}
```

这看起来很多，但实际上它只是一个用于构建 Qt 应用程序的相当标准的 CMake 文件。

这里的区别是：

```cmake,ignore
{{#include ../../../examples/qml_minimal/CMakeLists.txt:book_tutorial_cmake_diff_1}}
{{#include ../../../examples/qml_minimal/CMakeLists.txt:book_tutorial_cmake_diff_2}}
```

它将执行代码生成，并将其包含到 C++ 构建中。

这里要注意的重要一点是，CMake 必须能够解析 `include(CxxQt)`. 为此，您需要克隆 [CXX-Qt 代码库](https://github.com/KDAB/cxx-qt/)，并将 `CxxQt.cmake` 文件添加到 `CMAKE_MODULE_PATHCMake` 变量中。实现此目的的一种简单方法是使用 CMake 的 `-D` 选项。有关一些替代方案，请参阅 [CMake 概念章节](../concepts/cmake.md)。

因此构建我们的项目可以这样完成：

```shell
$ mkdir build && cd build
$ cmake -DCMAKE_MODULE_PATH="<path-to-cxx-qt-repo>/cmake" ..
$ cmake --build .
```

如果由于某种原因失败，请查看 [`examples/qml_minimal`](https://github.com/KDAB/cxx-qt/tree/main/examples/qml_minimal)文件夹，里面包含完整示例代码。

现在可以配置和编译我们的项目。如果这成功了，你现在可以运行我们的小项目了。

```shell
$ ./qml_minimal
```

您现在应该看到显示我们 `MyObject` 状态的两个 Label，以及调用我们的两个 Rust 函数的两个 Button。

## 成功   🥳

如需进一步阅读，您可以查看 [QObject 章节](../qobject/index.md)，该章节详细介绍了 CXX-Qt 向 QObject 子类公开的所有属性。也可以看[概念章节](../concepts/index.md)，它解释了 CXX-Qt 的底层概念。
