# 引用（&）和借用

## 引用（&）

 & 符号就是 **引用**，它们允许你使用值但不获取其所有权。

`&String s` 指向 `String s1` 示意图![&String s pointing at String s1](../../static/images/trpl04-05.svg)

> 与使用 `&` 引用相反的操作是 **解引用**（*dereferencing*），它使用解引用运算符，`*`。

```rust
let r1 = String::from("hello world");// 注意这里没有mut
let r2 = &r1;// 注意这里没有mut
```

>这个`&` 很神奇， 这样给 `r2` 赋值以后 不会产生 rust中的 `move` 操作
>这里表示的就是引用，**它们允许你使用值但不获取其所有权**

```rust
  let r1_addr = &r1 as *const String as usize;
  let r2_addr = r2 as *const String as usize;
  
  println!("r1 value = {} address is 0x{}",r1,r1_addr);
  println!("r2 value = {} address is 0x{}",r2,r2_addr);
```

>这里输出一下上面 `r1` 和 `r2` 的地址你会发现他们的地址是一致的
>这就证明他们指向了同一块内存空间。

## 借用

将获取引用作为函数参数称为**借用**

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1);

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize { // s 是对 String 的引用
    s.len()
} // 这里，s 离开了作用域。但因为它并不拥有引用值的所有权，
  // 所以什么也不会发生
```

## 可变引用

> 正如变量默认是不可变的，引用也一样。（默认）不允许修改引用的值。

尝试修改借用的值

```rust
fn main() {
    let s = String::from("hello");

    change(&s);
}

fn change(some_string: &String) {
    some_string.push_str(", world");
}
```

错误信息：

```shell
$ cargo run
   Compiling ownership v0.1.0 (file:///projects/ownership)
error[E0596]: cannot borrow `*some_string` as mutable, as it is behind a `&` reference
 --> src/main.rs:8:5
  |
7 | fn change(some_string: &String) {
  |                        ------- help: consider changing this to be a mutable reference: `&mut String`
8 |     some_string.push_str(", world");
  |     ^^^^^^^^^^^ `some_string` is a `&` reference, so the data it refers to cannot be borrowed as mutable

For more information about this error, try `rustc --explain E0596`.
error: could not compile `ownership` due to previous error

```

### 可变引用的使用

> Rust 中，要让一个变量可编辑，唯一的方式就是给他加上 `mut` 关键字。

引用的变量如果要变更，必须符合满足**三个**要求：

1. 变量本身是可变更的，也就是定义时必须添加 `mut` 关键字。
2. 函数的参数也必须定义为可变更的，加上 **借用 Borrowing** 或者说引用，也就是必须添加 `&mut` 关键字。
3. 传递 **借用 Borrowing** 或者说引用也必须声明时 **可变更传递**，也就是传递参数时必须添加 `&mut` 关键字。

```rust
fn main() {
    let mut s = String::from("hello");//--定义时添加 mut

    change(&mut s);//--传参时添加 &mut
}

fn change(some_string: &mut String) {//--定义时添加 &mut
    some_string.push_str(", world");
}

```

### 可变引用的限制

> 在**同一时间**，只能有**一个**对某一特定数据的可变引用。

这个限制的好处是 Rust 可以在编译时就避免数据竞争。**数据竞争**（*data race*）类似于竞态条件，它可由这三个行为造成：

- 两个或更多指针同时访问同一数据。
- 至少有一个指针被用来写入数据。
- 没有同步数据访问的机制。

数据竞争会导致未定义行为，难以在运行时追踪，并且难以诊断和修复；Rust 避免了这种情况的发生。

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &mut s;
    let r2 = &mut s;//--Error:cannot borrow `s` as mutable more than once at a time

    println!("{}, {}", r1, r2);
}
```

可以使用大括号来创建一个新的作用域，以允许拥有多个可变引用，只是不能 **同时** 拥有：

```rust
fn main() {
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

    let r2 = &mut s;
}
```

> 不能在拥有**不可变**引用的**同时**拥有**可变**引用。
>
> 然而，多个不可变引用是可以的。

注意一个引用的**作用域**从**声明**的地方开始一直持续到**最后一次使用**为止。

上面的意思是：可变引用只能**单独使用**，不能和其他的**引用**（包括变量**本身**）同时使用。

**错误示例**

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    let r3 = &mut s; // 大问题，Error：cannot borrow `s` as mutable because it is also borrowed as immutable

    println!("{}, {}, and {}", r1, r2, r3);
}
```

**正确示例**

```rust
fn main() {
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    println!("{} and {}", r1, r2);
    // 此位置之后 r1 和 r2 不再使用

    let r3 = &mut s; // 没问题
    println!("{}", r3);
}
```

## 引用的规则

> 在任意给定时间，**要么** 只能有一个可变引用，**要么** 只能有多个不可变引用。

> 引用必须总是有效的。

