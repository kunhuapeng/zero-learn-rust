# option 枚举

## 概述

- Option 枚举存在于标准库中, 位于**Prelude**(*预导入模块*) 中
- 描述某个值(数据类型)可能存在或者不存在的情况

```rust
// 标准库中的Option枚举
// <T> 表示泛型, 以后会讲
enum Option<T> {
    Some(T),
    None,
}
```

> `Option<T>` 枚举被包含在了 prelude 之中，不需要将其显式引入作用域。
>
> 另外，它的成员也是如此，可以不需要 `Option::` 前缀来直接使用 `Some` 和 `None`。

```rust
#![allow(unused)]
fn main() {
	let some_number = Some(5);
	let some_string = Some("a string");
    
	let absent_number: Option<i32> = None;
}
```

## 空值

使用 Option 枚举带表达空值

```rust
fn main() {
    let some_number: Option<i32> = Some(5);
    let absent_number: Option<i32> = None;
}
```

> 如果使用 `None` 而不是 `Some`，需要告诉 Rust `Option<T>` 是什么类型的，因为编译器只通过 `None` 值无法推断出 `Some` 成员保存的值的类型。

## 为什么用`Option<T>`表示空值

因为 `Option<T>` 和 `T`（这里 `T` 可以是任何类型）是不同的类型，编译器不允许像使用**一个肯定有效的值**那样 使用 `Option<T>`。

```rust
let x: i8 = 5;
let y: Option<i8> = Some(5);

let sum = x + y;
```

如果运行这些代码，将得到类似这样的错误信息：

```shell
error[E0277]: the trait bound `i8: std::ops::Add<std::option::Option<i8>>` is
not satisfied
 -->
  |
5 |     let sum = x + y;
  |                 ^ no implementation for `i8 + std::option::Option<i8>`
  |
```

 Rust 不知道该如何将 `Option<i8>` 与 `i8` 相加，因为它们的类型不同。

> 当在 Rust 中拥有一个像 `i8` 这样类型的值时，编译器确保它总是有一个有效的值。我们可以自信使用而无需做空值检查。
>
> 不再担心会错误的假设一个非空值。

只有当使用 `Option<i8>`（或者任何用到的类型）的时候需要担心可能没有值，而编译器会确保我们在使用值之前处理了为空的情况。

> 在对 `Option<T>` 进行 `T` 的运算之前必须将其转换为 `T`。

总的来说，为了使用 `Option<T>` 值，需要编写处理每个成员的代码。你想要一些代码只当拥有 `Some(T)` 值时运行，允许这些代码使用其中的 `T`。也希望一些代码在值为 `None` 时运行，这些代码并没有一个可用的 `T` 值。