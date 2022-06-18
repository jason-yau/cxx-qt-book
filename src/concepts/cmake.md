<!--
SPDX-FileCopyrightText: 2021 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# CMake

我们需要添加 CMake 来生成 C++ 代码，然后进行链接，在此要确保 `CxxQt.cmake` 可以被 CMake 找到。为此，必须调整[`CMAKE_MODULE_PATH` CMake 变量](https://cmake.org/cmake/help/latest/variable/CMAKE_MODULE_PATH.html)以包含 CXX-Qt 代码库中的 `cmake` 目录。

实现这一步骤的方法包括：

- 在调用 CMake 时提供 `-DCMAKE_MODULE_PATH=<path-to-cxx-qt-repo>/cmake` 选项。
- 添加 `list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/../cxx-qt/cmake")` 到 CXX-Qt 代码库的相对路径。
  - 如果将 CXX-Qt 作为 git 子模块添加到项目中，此选项特别有用。
- 使用 CMake GUI 更改变量

然后我们在 CMake 中有多个阶段要执行

  * `cxx_qt_generate_cpp`
    * 然后我们在 CMake 中有多个阶段要执行
    * 解析Rust 项目，以生成相关 C++ 代码
    * 将源代码添加到 `GEN_SOURCES`
  * `add_executable`
    * 将生成的 C++ 源代码添加到可执行文件中，和在普通 C++ 项目中一样
  * `cxx_qt_include`
    * 添加任何需要的 CXX-Qt 和 CXX 的静态源到包含目录中
  * `cxx_qt_link_rustlib`
    * 将静态 Rust 库链接到 C++ 目标

```cmake,ignore
{{#include ../../../examples/qml_minimal/CMakeLists.txt:book_cmake_generation}}
```

构建插件时，请参阅 [QQmlExtensionPlugin 页面](./qqmlextensionplugin.md)了解 CMake 的差异。
