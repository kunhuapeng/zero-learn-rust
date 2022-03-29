# 枚举 enums

## 概述

- **枚举**(*enumerations*)允许我们列举所有可能的值来定义一个类型
- 这些可能值被称为枚举的**成员**(*variants*) 有时称为变体

## 定义枚举

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

现在 `IpAddrKind` 就是一个可以在代码中使用的自定义数据类型了。

## 枚举值

```rust
let four = IpAddrKind::V4;
let six = IpAddrKind::V6;
```

> 枚举的变体都位于标识符的命名空间下, 使用户`::`进行访问

这么设计的益处是现在 `IpAddrKind::V4` 和 `IpAddrKind::V6` 都是 `IpAddrKind` 类型的。

## 枚举值中存数据

```rust
enum IpAddrKind {
    V4(u8,u8,u8,u8),
    V6(String),
}

fn main() {
    let _four = IpAddrKind::V4(127, 0, 0, 1);
    let _six = IpAddrKind::V6(String:from("::1"));
}
```

用枚举替代结构体还有另一个优势：每个成员可以处理不同类型和数量的数据。

**示例**

```rust
#![allow(unused)]
fn main() {
    enum Message {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }
}
```

这个枚举有四个含有不同类型的成员：

- `Quit` 没有关联任何数据。
- `Move` 包含一个匿名结构体。
- `Write` 包含单独一个 `String`。
- `ChangeColor` 包含三个 `i32`。

## 枚举的方法

```rust
#![allow(unused)]
fn main() {
    enum Message {
        Quit,
        Move { x: i32, y: i32 },
        Write(String),
        ChangeColor(i32, i32, i32),
    }

    impl Message {
        fn call(&self) {
            // 在这里定义方法体
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
}
```

> 和结构体方法的编写一样

方法体使用了 `self` 来获取调用方法的值。这个例子中，创建了一个值为 `Message::Write(String::from("hello"))` 的变量 `m`，而且这就是当 `m.call()` 运行时 `call` 方法中的 `self` 的值。