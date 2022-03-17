# loop 循环

## 概述

- `loop` 循环是一个死循环
- 需要我么显示的调用 `break` 结束循环
- 可使用 `continue` 跳过本次循环
- 可以在 `let` 语句的右侧使用它来将结果赋值给一个变量。

## loop

`loop` 关键字告诉 Rust 一遍又一遍地执行一段代码直到你明确要求停止。

```rust
fn main() {
    loop {
        println!("again!");//--这是个死循环，一直在打印again!
    }
}
```

当运行这个程序时，我们会看到连续的反复打印 `again!`，直到我们手动停止程序。大部分终端都支持一个快捷键 ctrl-c 来终止一个陷入无限循环的程序。

## break

可以使用 `break` 关键字来告诉程序何时停止循环。

```rust
fn main() {
    let mut i = 3;
    loop {
        if i == 0 {
            break;
        }
        println!("输出: {}", i);
        i -= 1;
    }
}
```

## continue

循环中的 `continue` 关键字告诉程序跳过这个循环迭代中的**任何剩余代码**，并转到下一个迭代。

```rust
fn main() {
    let mut i = 3;
    loop {
        if i == 0 {
            break;
        }
        if i == 2 {
            continue;
        }
        println!("输出: {}", i);
        i -= 1;
    }
}
```

## loop label

> 如果存在嵌套循环，`break` 和 `continue` 应用于此时**最内层**的循环。

可以选择在一个循环上指定一个**循环标签**（*loop label*），然后将标签与 `break` 或 `continue` 一起使用，使这些关键字应用于已标记的循环而不是最内层的循环。

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {}", count);
        let mut remaining = 10;

        loop {
            println!("remaining = {}", remaining);
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {}", count);
}
```

## let-loop

可以在用于停止循环的 `break` 表达式添加你想要返回的值；该值将从循环中返回，以便用变量接收。

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            break counter * 2;
        }
    };

    println!("The result is {}", result);
}
```

