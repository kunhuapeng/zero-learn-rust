# 布尔类型

**布尔类型** 只有两个可能的取值 `true` 或 `false` 。

Rust 使用 `bool` 关键字来声明一个 **布尔类型** 的变量。

> 布尔值的大小为 1 个字节。

## 布尔类型声明

```rust
fn main() {
   let isfun:bool = true;
   println!("Is Rust Programming Fun ? {}",isfun);
}
```

编译运行上面的代码，输出结果如下

```shell
Is Rust Programming Fun ? true
```

使用布尔值的主要地方是条件判断，如 `if` 表达式。