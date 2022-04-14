# 模式匹配（patterns）

## 模式匹配

模式是 Rust 中特殊的语法，它用来匹配类型中的结构，无论类型是简单还是复杂。模式不仅用于将值与结构匹配，也可将内部的值绑定给变量。模式还用于变量定义（Rust的变量定义其实只是模式的一种应用），函数和方法的参数定义，闭包的参数定义。

```rust
if let
     Person {
         car: Some(_),
         age: person_age @ 13..=19,
         name: ref person_name,
         ..
     } = person
 {
     println!("{} has a car and is {} years old.", person_name, person_age);
 }
```

以上例子出现了1种隐式匹配：

- 判断person是不是一个Person结构体，如果不是就全部不满足了

4种显式匹配：

- 判断person的car字段是不是非None的值（即Some(_)），但不关心具体的值
- 判断person的age字段是不是在[13, 19]范围内（前后闭区间），并且将值绑定给person_age变量
- 将person的name字段的引用绑定给变量person_name
- 忽视person的所有其他字段，即剩余字段可以有任何值

## 定义及出没场所

模式是 Rust 中特殊的语法，它用来匹配类型中的结构，无论类型是简单还是复杂。模式不仅用于将值与结构匹配，也可将内部的值绑定给变量。模式还用于变量定义（Rust的变量定义其实只是模式的一种应用），函数和方法的参数定义，闭包的参数定义。模式常见于如下场景：

- let 语句
- 函数与闭包参数
- match 表达式
- if let 表达式
- while let 表达式
- for 表达式

## 匹配会失效吗

模式匹配有如下两种：

`refutable`：可反驳的，简而言之就是匹配可能失败

`irrefutable`：不可反驳的，简而言之就是匹配一定成功

```rust
 let (x, y) = (1, 2);               // "(x, y)" is an irrefutable pattern
 
 if let (a, 3) = (1, 2) {           // "(a, 3)" is refutable, and will not match
     panic!("Shouldn't reach here");
 } else if let (a, 4) = (3, 4) {    // "(a, 4)" is refutable, and will match
     println!("Matched ({}, 4)", a);
 }
```

## 解构（destructuring）

模式可以用来解构 `structs`，`enums`，`tuples`。解构将值拆成一个个它的组成部分。这个解构语法与创建值的语法基本是一样的。如果模式的检查表达式里有`structs`，`enums`，`tuples`，那么单下划线 (`_`) 表示单个字段的泛匹配，(`..`)表示剩余所有字段的泛匹配，在解构一个数据体的是，不允许使用方式：`fieldname` 来表达 `fieldname: fieldname`（这个方式在创建变量的时候是有效的）

```rust
 match message {
     Message::Quit => println!("Quit"),
     Message::WriteString(write) => println!("{}", &write),
     Message::Move{ x, y: 0 } => println!("move {} horizontally", x),
     Message::Move{ .. } => println!("other move"),
     Message::ChangeColor { 0: red, 1: green, 2: _ } => {
         println!("color change, red: {}, green: {}", red, green);
     }
 };
```

