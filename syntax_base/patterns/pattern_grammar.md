# 模式语法

## 模式语法

```yaml
 Pattern :
    | LiteralPattern
    | IdentifierPattern
    | WildcardPattern
    | RangePattern
    | ReferencePattern
    | StructPattern
    | TupleStructPattern
    | TuplePattern
    | GroupedPattern
    | SlicePattern
    | PathPattern
    | MacroInvocation
```

## 匹配字面量 LiteralPattern

**语法**

```yaml
LiteralPattern :
    | BOOLEAN_LITERAL
    | CHAR_LITERAL
    | BYTE_LITERAL
    | STRING_LITERAL
    | RAW_STRING_LITERAL
    | BYTE_STRING_LITERAL
    | RAW_BYTE_STRING_LITERAL
    | -? INTEGER_LITERAL
    | -? FLOAT_LITERAL
```

Literal模式实现精确匹配。如果希望代码获得特定的具体值，则该语法很有用。

```rust
 for i in -2..5 {
     match i {
         -1 => println!("It's minus one"),
         1 => println!("It's a one"),
         2|4 => println!("It's either a two or a four"),
         _ => println!("Matched none of the arms"), 
     }
 }
```

### 多个模式

在 `match` 表达式中，可以使用 `|` 语法匹配多个模式，它代表 **或**（*or*）的意思。例如，如下代码将 `x` 的值与匹配分支相比较，第一个分支有 **或** 选项，意味着如果 `x` 的值匹配此分支的任一个值，它就会运行：

```rust
#![allow(unused)]
fn main() {
    let x = 1;

    match x {
        1 | 2 => println!("one or two"),
        3 => println!("three"),
        _ => println!("anything"),
    }
}
```

## 匹配命名变量 IdentifierPattern

**语法**

```yaml
 IdentifierPattern :
       ref? mut? IDENTIFIER (@ Pattern ) ?
```

Identifier模式，将匹配到的值绑定给某个变量，这个identifier在这个模式中应该是唯一的。这个变量会shadow在这个作用于中出现的同名变量。这个新绑定的变量的作用于范围与模式所处的上下文有关（比如let绑定 或 match arm的作用域范围就不一样）

命名变量是匹配任何值的不可反驳模式。然而当其用于 `match` 表达式时情况会有些复杂。因为 `match` 会开始一个新作用域，`match` 表达式中作为模式的一部分声明的变量会覆盖 `match` 结构之外的同名变量，与所有变量一样。

```rust
fn main() {
    let x = Some(5);
    let y = 10;

    match x {
        Some(50) => println!("Got 50"),
        Some(y) => println!("Matched, y = {:?}", y),
        _ => println!("Default case, x = {:?}", x),
    }

    println!("at the end: x = {:?}, y = {:?}", x, y);
}
```

第一个匹配分支的模式并不匹配 `x` 中定义的值，所以代码继续执行。

第二个匹配分支中的模式引入了一个新变量 `y`，它会匹配任何 `Some` 中的值。因为我们在 `match` 表达式的新作用域中，这是一个新变量，而不是开头声明为值 10 的那个 `y`。这个新的 `y` 绑定会匹配任何 `Some` 中的值，在这里是 `x` 中的值。因此这个 `y` 绑定了 `x` 中 `Some` 内部的值。这个值是 5，所以这个分支的表达式将会执行并打印出 `Matched, y = 5`。

如果 `x` 的值是 `None` 而不是 `Some(5)`，头两个分支的模式不会匹配，所以会匹配下划线。这个分支的模式中没有引入变量 `x`，所以此时表达式中的 `x` 会是外部没有被覆盖的 `x`。在这个假想的例子中，`match` 将会打印 `Default case, x = None`。

一旦 `match` 表达式执行完毕，其作用域也就结束了，同理内部 `y` 的作用域也结束了。最后的 `println!` 会打印 `at the end: x = Some(5), y = 10`。

> **在identifier模式中，绑定一个变量可能会发生值的copy或者move，取决于这个值的类型是否实现了`Copy`**

## 范围匹配 Range patterns

**语法**

```yaml
 RangePattern :
    | RangePatternBound ..= RangePatternBound
    | RangePatternBound ... RangePatternBound
 
 RangePatternBound :
    | CHAR_LITERAL
    | BYTE_LITERAL
    | -? INTEGER_LITERAL
    | -? FLOAT_LITERAL
    | PathInExpression
    | QualifiedPathInExpression
```

`..=` 语法允许你匹配一个闭区间范围内的值。

```rust

#![allow(unused)]
fn main() {
    let x = 5;

    match x {
        1..=5 => println!("one through five"),
        _ => println!("something else"),
    }
}
```

> 范围只允许用于数字或 `char` 值，因为编译器会在编译时检查范围不为空。`char` 和 数字值是 Rust 仅有的可以判断范围是否为空的类型。

Range模式允许你匹配一个闭区间范围内的值，比如pattern `'m'..='p'` 会匹配 `'m'`, `'n'`, `'o'`, and `'p'`，当前允许为literals或paths，在这个 `a..=b`模式下必须有`a<=b`。`a...b`语法是为了保持向后兼容而留下的。这个模式当前只是用于如下scalar types。（请注意，这里与`for`语句中出现的range表达式不一致，for语句中使用`a..b`代表[a, b)，这里是`a...b`代表[a, b) ）

一个使用 `char` 类型值范围的例子

```rust
#![allow(unused)]
fn main() {
    let x = 'c';

    match x {
        'a'..='j' => println!("early ASCII letter"),
        'k'..='z' => println!("late ASCII letter"),
        _ => println!("something else"),
    }
}
```

## 解构并分解值

也可以使用模式来解构结构体、枚举、元组和引用，以便使用这些值的不同部分。

### 解构结构体 Struct patterns

可以通过带有模式的 `let` 语句将结构体分解

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x: a, y: b } = p;
    assert_eq!(0, a);
    assert_eq!(7, b);
}
```

这段代码创建了变量 `a` 和 `b` 来匹配结构体 `p` 中的 `x` 和 `y` 字段。这个例子展示了模式中的变量名不必与结构体中的字段名一致。不过通常希望变量名与字段名一致以便于理解变量来自于哪些字段。

> 因为变量名匹配字段名是常见的，同时因为 `let Point { x: x, y: y } = p;` 包含了很多重复，所以对于匹配结构体字段的模式存在简写：只需列出结构体字段的名称，则模式创建的变量会有相同的名称。

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    let Point { x, y } = p;
    assert_eq!(0, x);
    assert_eq!(7, y);
}
```

> 也可以使用字面量作为结构体模式的一部分进行解构，而不是为所有的字段创建变量。这允许我们测试一些字段为特定值的同时创建其他字段的变量。

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 0, y: 7 };

    match p {
        Point { x, y: 0 } => println!("On the x axis at {}", x),
        Point { x: 0, y } => println!("On the y axis at {}", y),
        Point { x, y } => println!("On neither axis: ({}, {})", x, y),
    }
}
```

第一个分支通过指定字段 `y` 匹配字面量 `0` 来匹配任何位于 `x` 轴上的点。此模式仍然创建了变量 `x` 以便在分支的代码中使用。

类似的，第二个分支通过指定字段 `x` 匹配字面量 `0` 来匹配任何位于 `y` 轴上的点，并为字段 `y` 创建了变量 `y`。第三个分支没有指定任何字面量，所以其会匹配任何其他的 `Point` 并为 `x` 和 `y` 两个字段创建变量。

在这个例子中，值 `p` 因为其 `x` 包含 0 而匹配第二个分支，因此会打印出 `On the y axis at 7`。
