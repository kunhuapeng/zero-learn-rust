# 浮点类型

## 浮点数

**浮点数**（*floating-point number*）是带有小数点的数字，在 Rust 中浮点类型（简称浮点型）数字也有两种基本类型。

> Rust 的浮点型是 `f32` 和 `f64`，它们的大小分别为 32 位和 64 位。
>
> **默认**浮点类型是 `f64`，因为在现代的 CPU 中它的速度与 `f32` 的几乎相同，但精度更高。
>
> 所有浮点型都是**有符号**的。

按照存储大小，我们把浮点型划分为 `f32` 和 `f64`。其中 `f64` 是默认的浮点类型。

- `f32` 又称为 **单精度浮点型**。
- `f64` 又称为 **双精度浮点型**，它是 Rust **默认**的浮点类型.

## 浮点型与整型的区别

> Rust 区分整型和浮点型的唯一指标就是 **有没有小数点**。

> Rust 中的整型和浮点型是严格区分的，不能相互转换。

也就是说，我们不能将 `0.0` 赋值给任意一个整型，也不能将 `0` 赋值给任意一个浮点型。

## 定义浮点型的变量

```rust
fn main() {
   let result = 10.00;        // 默认是 f64 
   let interest:f32 = 8.35;
   let cost:f64 = 15000.600;  // 双精度浮点型

   println!("result value is {}",result);
   println!("interest is {}",interest);
   println!("cost is {}",cost);
}
```

## 不允许类型自动转换

Rust 中的数字类型与 C/C++ 中不同的是 **Rust 语言不允许类型自动转换**。

例如，把一个 **整型** 赋值给一个 **浮点型** 是会报错的。

```rust
fn main() {
   let interest:f32 = 8;   // integer assigned to float variable
   println!("interest is {}",interest);
}
```

编译上面的代码，会抛出 `mismatched types error` 错误

```shell
error[E0308]: mismatched types
   --> main.rs:2:22
   |
 2 | let interest:f32=8;
   |    ^ expected f32, found integral variable
   |
   = note: expected type `f32`
      found type `{integer}`
error: aborting due to previous error(s)
```

## 数字可读性分隔符 `_`

为了方便阅读超大的数字，Rust 语言允许使用一个 **虚拟的分隔符** 也就是 **下划线（ `_` ）** 来对数字进行可读性分隔符。

比如为了提高 `50000` 的可读性，我们可以写成 `50_000` 。

> Rust 语言会在编译时移除数字可读性分隔符 `_`
>
> 整型和浮点型**都可以使用**数字可读性分隔符 `_`

**范例：**

```rust
fn main() {
   let float_with_separator = 11_000.555_001;
   println!("float value {}",float_with_separator);

   let int_with_separator = 50_000;
   println!("int value {}",int_with_separator);
}
```

编译运行上面的代码，输出结果如下

```shell
float value 11000.555001
int value 50000
```

