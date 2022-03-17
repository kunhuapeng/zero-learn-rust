# `if`表达式

## 概述

- if 表达式允许您根据所提供的条件来执行不同的代码分支
- 在 if 表达式中, 与条件相关的代码块就做 **分支**(*arm*)
- 可选: 可以在 if 表达式之后跟随 else 表达式
- 可以将 `if` 和 `else` 组成的 `else if` 表达式来实现多重条件。
- 可以在 `let` 语句的右侧使用它来将结果赋值给一个变量。

**示例**

```rust
fn main() {
    let num = 3;
    
    // if-else表达式
    if num < 5 {
        println!("条件为 true");
    } else {
        println!("条件为 false");
    }
    
    // if-else-if 表达式
    if num > 0 {
        println!("正数");
    } else if num == 0 {
        println!("0");
    } else {
         println!("负数");
    }
    
    // 既然是表达式, 那么它就可以被变量锁接收
    let result = if num >= 0 {
        "非负数数"
    } else {
        "负数"
    };
    println!("{} 是 {}", num, result);
}
```

## if表达式

`if` 表达式允许根据条件执行不同的代码分支。

所有的 `if` 表达式都以 `if` 关键字开头，其后跟一个条件。

`if` 表达式中与条件关联的代码块有时被叫做**分支**（*arm*）

> 值得注意的是代码中的条件**必须**是 `bool` 值。如果条件不是 `bool` 值，我们将得到一个错误。

> Rust 并不会尝试自动地将非布尔值转换为布尔值。你必须自始至终显式地使用**布尔值**作为 `if` 的条件。

```rust
fn main() {
    let number = 3;

    if number < 5 {
        println!("condition was true");
    }
}
```

## if-else表达式

也可以包含一个可选的 `else` 表达式来提供一个在条件为假时应当执行的代码块。

如果不提供 `else` 表达式并且条件为假时，程序会直接**忽略** `if` 代码块并继续执行下面的代码。

```rust
fn main() {
    let number = 5;

    if number < 5 {
        println!("condition was true");
    } else {
        println!("condition was false");
    }
}
```

## if-else-if 表达式

可以将 `if` 和 `else` 组成的 `else if` 表达式来实现多重条件。

```rust
fn main() {
    let number = 6;

    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

它按顺序检查每个 `if` 表达式并执行第一个条件为真的代码块。注意即使 6 可以被 2 整除，也不会输出 `number is divisible by 2`，更不会输出 `else` 块中的 `number is not divisible by 4, 3, or 2`。原因是 Rust 只会执行第一个条件为真的代码块，并且一旦它找到一个以后，甚至都不会检查剩下的条件了。

使用过多的 `else if` 表达式会使代码显得杂乱无章，所以如果有多于一个 `else if` 表达式，最好使用`match`。

## let-if 表达式

 `if` 是一个表达式，我们可以在 `let` 语句的右侧使用它来将结果赋值给一个变量。

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {}", number);
}
```

> 代码块的值是其最后一个表达式的值

> 整个 `if` 表达式的值取决于哪个代码块被执行。

> `if` 的每个分支的可能的返回值都必须是相同类型

