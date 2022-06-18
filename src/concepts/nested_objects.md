<!--
SPDX-FileCopyrightText: 2022 Klarälvdalens Datakonsult AB, a KDAB Group company <info@kdab.com>
SPDX-FileContributor: Andrew Hayzen <andrew.hayzen@kdab.com>

SPDX-License-Identifier: MIT OR Apache-2.0
-->

# 嵌套对象

Rust Qt 对象可以作为彼此的属性或参数嵌套。

嵌套对象由它的 `crate` 相对路径引用，然后 `CppObj` 作为最后一个参数。如 `crate::mymod::secondary_object::CppObj` 指的是一个 `mymod.rs` 包含带有 [CXX-Qt 宏](../qobject/macro.md)的 `secondary_object` 的模块。

要将其用作为另一个对象的属性时，请以 `secondary_object: crate::mymod::secondary_object::CppObj` 作为属性。

用作可调函数用参数时，请以 `secondary_object: &mut crate::mymod::secondary_object::CppObj` 作为参数。然后可以通过常规 `CppObj` 方法使用该参数 `secondary_object`。

以下是一个将嵌套对象显示为属性和参数的示例。

```rust,ignore,noplayground
{{#include ../../../examples/qml_features/src/nested.rs:book_macro_code}}
```

注意：嵌套对象还不能用作返回类型( [https://github.com/KDAB/cxx-qt/issues/66](https://github.com/KDAB/cxx-qt/issues/66) )。

注意：嵌套对象在（反）序列化中会被忽略 ( [https://github.com/KDAB/cxx-qt/issues/35](https://github.com/KDAB/cxx-qt/issues/35) )。

注意：嵌套对象不能在信号中使用 ( [https://github.com/KDAB/cxx-qt/issues/73](https://github.com/KDAB/cxx-qt/issues/73) )。

注意：在未来可能允许使用 `super::` ( [https://github.com/KDAB/cxx-qt/issues/44](https://github.com/KDAB/cxx-qt/issues/44) )。

TODO: 一旦我们有了 borrow_rust_obj() ，请说明它访问其他对象 RustObj 的目的 [https://github.com/KDAB/cxx-qt/issues/30](https://github.com/KDAB/cxx-qt/issues/30) )。
