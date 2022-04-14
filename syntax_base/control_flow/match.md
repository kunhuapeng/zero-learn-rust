# `match`表达式

## match 控制流运算符

Rust 有一个叫做 `match` 的极为强大的控制流运算符，它允许我们将一个值与一系列的模式相比较，并根据相匹配的模式执行相应代码。模式可由字面量、变量、通配符和许多其他内容构成；

### 语法格式

```rust
let expressionResult = match variable_expression {
   constant_expr1 => {
      // 语句;
   },
   constant_expr2 => {
      // 语句;
   },
   _ => {
      // 默认
      // 其它语句
   }
};
```

1. 首先要说明的是 **match 关键字后面的表达式不必括在括号中**。也就是 `variable_expression` 不需要用一对 **括号(`()`)** 括起来。

2. 其次，`match` 语句在执行的时候，会计算 `variable_expression` 表达式的值，然后把计算后的结果和每一个 `constant_exprN` 匹配，使用的是 **全等于** 也就是 `===` 来匹配。如果匹配成功则执行 `=> {}` 里面的语句。

3. 如果 `variable_expression` 表达式的值没有和任何一个 `constant_exprN` 匹配，那么它会默认匹配 `_`。

   因此，当没有匹配时，默认会执行 `_ => {}` 中的语句。

4. `match` 语句有返回值，它把 **匹配值** 后执行的最后一条语句的结果当作返回值。

5. `_ => {}` 语句是可选的，也就是说 `match` 语句可以没有它。

6. 如果只有一条语句，那么每个 `constant_expr2 => {}` 中的 `{}` 是可以省略的。

**示例**

```rust
enum Direction {
    East,
    West,
    North,
    South,
}
fn main() {
    let dire = Direction::South;
    match dire {
        Direction::East => println!("East"),
        Direction::North | Direction::South => {
            println!("South or North");
        },
        _ => println!("West"),
    };
}
```

当 `match` 表达式执行时，它将结果值按顺序与每一个分支的模式相比较。如果模式匹配了这个值，这个模式相关联的代码将被执行。如果模式并不匹配这个值，将继续执行下一个分支。

> match所罗列的匹配，必须穷举出其所有可能。可以用 **_** 这个符号来代表其余的所有可能性情况。

### 表达式的返回值

```rust
enum Direction {
    East,
    West,
    North,
    South,
}
fn main() {
    // let d_panic = Direction::South;
    let d_west = Direction::West;
    let d_str = match d_west {
        Direction::East => "East",
        Direction::North | Direction::South => {
            panic!("South or North");
        },
        _ => "West",
    };
    println!("{}", d_str);
}
```

> `match` 语句有返回值，它把 **匹配值** 后执行的最后一条语句的结果当作返回值。

> match的每一个分支都必须是一个表达式，并且，除非一个分支一定会触发panic，这些分支的所有表达式的最终返回值类型必须相同。

> 这个结果可以被赋予一个变量。

### 循环中的match

```rust
fn main(){
    let xs = [1,2,3,4,5];
    for x in &xs{
        let xx = match x{
            1 => "is 1",
            _ => continue,
        };
        println!("x = {}", x);
        println!("xx = {}", xx)
    }
}
 
/*
运行结果
x = 1
xx = is 1
*/
```

> 这个continue，是不违反语法的。如果把continue换成println!("not 1")会报错，换成break，不报错。