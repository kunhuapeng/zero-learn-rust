# 字符类型（char）

## 字符

**字符** ，简单的来说，就是字符串的基本组成部分，也就是单个字符或字。

> Rust 使用 `char` 作为 **字符数据类型**。这点可谓是继承了 C / C++。
> 但与 C / C++ 不同的是：**Rust 使用 UTF-8 作为底层的编码** ，而不是常见的使用 ASCII 作为底层编码。
> 也就是说，Rust 中的 **字符数据类型** 包含了 **数字**、**字母**、**Unicode** 和 **其它特殊字符**。

Unicode 编码的标量值的范围从 `U+0000` 到 `U+D7FF`， `U+E000` 到 `U+10FFFF`（含）。

> Rust 的字符类型大小为 4 个字节，表示的是一个 Unicode 标量值。

## 字符声明：

```rust
fn main() {
   let special_character = '@'; //default
   let alphabet:char = 'A';
   let emoji:char = 'c'; // 笑脸的那个图

   println!("special character is {}",special_character);
   println!("alphabet is {}",alphabet);
   println!("emoji is {}",emoji);
}
```

> 我们声明的 `char` 字面量采用**单引号**括起来。
>
> 这和字符串字面不同，字符串字面量是用**双引号**扩起来。

```rust
fn main() {	
	let a = "a";//&str
    let b = 'b';//char
    println!("{}{}", a, b);
}
```

