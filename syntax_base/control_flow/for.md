# for 循环

## 概述

- for 循环是迭代器循环
- 可使用 break 提前结束循环
- 可使用 continue 跳过本次循环

`for` 语句是一种能确定循环次数的循环。

`for` 语句用于执行代码块指定的次数。

Rust 中的 `for` 循环只有 `for..in` 这种格式，常用于迭代一组固定的值，例如数组、向量等。

## 语法格式

```rust
for var in iterator {
    code
}
```

> `var` 是循环迭代的每一个元素。

> `var` 变量的作用域仅限于 `for...in` 语句，超出则会报错

Rust 迭代器返回一系列的元素，每个元素是循环中的一次重复。然后它的值与 `var` 绑定，它在循环体中有效。每当循环体执行完后，我们从迭代器中取出下一个值，然后我们再重复一遍。当迭代器中不再有值时，`for` 循环结束。

## 遍历 Range 

`for` 循环的安全性和简洁性使得它成为 Rust 中使用最多的循环结构。即使是在想要循环执行代码特定次数时，大部分 Rustacean 也会使用 `for` 循环。这么做的方式是使用 `Range`，它是标准库提供的类型，用来生成从一个数字开始到另一个数字之前结束的所有数字的序列。

```rust
for x in 0..10 {
    println!("{}", x); // x: i32
}
```

上述迭代器的形式虽好，但是好像在循环过程中，少了索引信息。Rust 考虑到了这一点，当你需要记录你已经循环了多少次了的时候，你可以使用 `.enumerate()` 函数。

## 索引

```rust
for (i,j) in (5..10).enumerate() {
    println!("i = {} and j = {}", i, j);
}
```

输出：

```shell
i = 0 and j = 5
i = 1 and j = 6
i = 2 and j = 7
i = 3 and j = 8
i = 4 and j = 9
```

```rust
let lines = "Content of line one
Content of line two
Content of line three
Content of line four".lines();
for (linenumber, line) in lines.enumerate() {
    println!("{}: {}", linenumber, line);
}
```

输出：

```shell
0: Content of line one
1: Content of line two
2: Content of line three
3: Content of line four
```

