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

### 解构枚举 Tuple patterns

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

fn main() {
    let msg = Message::ChangeColor(0, 160, 255);

    match msg {
        Message::Quit => {
            println!("The Quit variant has no data to destructure.")
        }
        Message::Move { x, y } => {
            println!(
                "Move in the x direction {} and in the y direction {}",
                x,
                y
            );
        }
        Message::Write(text) => println!("Text message: {}", text),
        Message::ChangeColor(r, g, b) => {
            println!(
                "Change the color to red {}, green {}, and blue {}",
                r,
                g,
                b
            )
        }
    }
}
```

这段代码会打印出 `Change the color to red 0, green 160, and blue 255`。尝试改变 `msg` 的值来观察其他分支代码的运行。

对于像 `Message::Quit` 这样没有任何数据的枚举成员，不能进一步解构其值。只能匹配其字面量 `Message::Quit`，因此模式中没有任何变量。

对于像 `Message::Move` 这样的类结构体枚举成员，可以采用类似于匹配结构体的模式。在成员名称后，使用大括号并列出字段变量以便将其分解以供此分支的代码使用。这里使用了示例 所展示的简写。

对于像 `Message::Write` 这样的包含一个元素，以及像 `Message::ChangeColor` 这样包含三个元素的类元组枚举成员，其模式则类似于用于解构元组的模式。

> 模式中变量的数量必须与成员中元素的数量一致。

### 解构嵌套的结构体和枚举 Tuple struct patterns

所有的例子都只匹配了深度为一级的结构体或枚举。当然也可以匹配嵌套的项！

```rust
enum Color {
   Rgb(i32, i32, i32),
   Hsv(i32, i32, i32),
}

enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(Color),
}

fn main() {
    let msg = Message::ChangeColor(Color::Hsv(0, 160, 255));

    match msg {
        Message::ChangeColor(Color::Rgb(r, g, b)) => {
            println!(
                "Change the color to red {}, green {}, and blue {}",
                r,
                g,
                b
            )
        }
        Message::ChangeColor(Color::Hsv(h, s, v)) => {
            println!(
                "Change the color to hue {}, saturation {}, and value {}",
                h,
                s,
                v
            )
        }
        _ => ()
    }
}
```

`match` 表达式第一个分支的模式匹配一个包含 `Color::Rgb` 枚举成员的 `Message::ChangeColor` 枚举成员，然后模式绑定了 3 个内部的 `i32` 值。第二个分支的模式也匹配一个 `Message::ChangeColor` 枚举成员， 但是其内部的枚举会匹配 `Color::Hsv` 枚举成员。我们可以在一个 `match` 表达式中指定这些复杂条件，即使会涉及到两个枚举。

甚至可以用复杂的方式来混合、匹配和嵌套解构模式。如下是一个复杂结构体的例子，其中结构体和元组嵌套在元组中，并将所有的原始类型解构出来：

```rust
#![allow(unused)]
fn main() {
    struct Point {
        x: i32,
        y: i32,
    }

    let ((feet, inches), Point {x, y}) = ((3, 10), Point { x: 3, y: -10 });
}
```

## 忽略模式中的值 Wildcard pattern

有时忽略模式中的一些值是有用的，比如 `match` 中最后捕获全部情况的分支实际上没有做任何事，但是它确实对所有剩余情况负责。

```rust
 let (a, _) = (10, x);   // the x is always matched by _
 
 // ignore a function/closure param
 let real_part = |a: f64, _: f64| { a };
 
 // ignore a field from a struct
 let RGBA{r: red, g: green, b: blue, a: _} = color;
 
 // accept any Some, with any value
 if let Some(_) = x {}
```

> 有一些简单的方法可以忽略模式中全部或部分值：使用 `_` 模式，在另一个模式中使用 `_` 模式，使用一个以下划线开始的名称，或者使用 `..` 忽略所剩部分的值。

### 使用 _ 忽略整个值

```rust
fn foo(_: i32, y: i32) {
    println!("This code only uses the y parameter: {}", y);
}

fn main() {
    foo(3, 4);
}
```

> `_` 模式作为 `match` 表达式最后的分支特别有用，也可以将其用于任意模式，包括函数参数中

这段代码会完全忽略作为第一个参数传递的值 `3`，并会打印出 `This code only uses the y parameter: 4`。

大部分情况当你不再需要特定函数参数时，最好修改签名不再包含无用的参数。在一些情况下忽略函数参数会变得特别有用，比如实现 trait 时，当你需要特定类型签名但是函数实现并不需要某个参数时。此时编译器就不会警告说存在未使用的函数参数，就跟使用命名参数一样。

### 使用嵌套的 _ 忽略部分值

也可以在一个模式内部使用 `_` 忽略部分值，例如，当只需要测试部分值但在期望运行的代码中没有用到其他部分时。

```rust
#![allow(unused)]
fn main() {
    let mut setting_value = Some(5);
    let new_setting_value = Some(10);

    match (setting_value, new_setting_value) {
        (Some(_), Some(_)) => {
            println!("Can't overwrite an existing customized value");
        }
        _ => {
            setting_value = new_setting_value;
        }
    }

    println!("setting is {:?}", setting_value);
}
```

> 当不需要 Some 中的值时在模式内使用下划线来匹配 Some 成员

也可以在一个模式中的多处使用下划线来忽略特定值

```rust
#![allow(unused)]
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        (first, _, third, _, fifth) => {
            println!("Some numbers: {}, {}, {}", first, third, fifth)
        },
    }
}
```

这里忽略了一个五元元组中的第二和第四个值

### 忽略未使用的变量

如果你创建了一个变量却不在任何地方使用它, Rust 通常会给你一个警告，因为这可能会是个 bug。但是有时创建一个还未使用的变量是有用的，比如你正在设计原型或刚刚开始一个项目。这时你希望告诉 Rust 不要警告未使用的变量，为此可以用下划线作为变量名的开头。

> 通过在名字前以一个下划线开头来忽略未使用的变量

```rust
fn main() {
    let _x = 5;
    let y = 10;
}
```

示例中创建了两个未使用变量，不过当运行代码时只会得到其中一个的警告

> 只使用 `_` 和使用以下划线开头的名称有些微妙的不同：比如 `_x` 仍会将值绑定到变量，而 `_` 则完全不会绑定。

```rust
let s = Some(String::from("Hello!"));

if let Some(_s) = s {
    println!("found a string");
}

println!("{:?}", s);
```

我们会得到一个错误，因为 `s` 的值仍然会移动进 `_s`，并阻止我们再次使用 `s`。然而只使用下划线本身，并不会绑定值。

```rust
#![allow(unused)]
fn main() {
    let s = Some(String::from("Hello!"));

    if let Some(_) = s {
        println!("found a string");
    }

    println!("{:?}", s);
}
```

上面的代码能很好的运行；因为没有把 `s` 绑定到任何变量；它没有被移动。

### 用 .. 忽略剩余值

对于有多个部分的值，可以使用 `..` 语法来只使用部分并忽略其它值，同时避免不得不每一个忽略值列出下划线。

> `..` 模式会忽略模式中剩余的任何没有显式匹配的值部分。

```rust
#![allow(unused)]
fn main() {
    struct Point {
        x: i32,
        y: i32,
        z: i32,
    }

    let origin = Point { x: 0, y: 0, z: 0 };

    match origin {
        Point { x, .. } => println!("x is {}", x),
    }
}
```

在示例中，有一个 `Point` 结构体存放了三维空间中的坐标。在 `match` 表达式中，我们希望只操作 `x` 坐标并忽略 `y` 和 `z` 字段的值。

这里列出了 `x` 值，接着仅仅包含了 `..` 模式。这比不得不列出 `y: _` 和 `z: _` 要来得简单，特别是在处理有很多字段的结构体，但只涉及一到两个字段时的情形。

```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        (first, .., last) => {
            println!("Some numbers: {}, {}", first, last);
        },
    }
}
```

这是元组中 `..` 的应用

>使用 `..` 必须是无歧义的。如果期望匹配和忽略的值是不明确的，Rust 会报错。

```rust
fn main() {
    let numbers = (2, 4, 8, 16, 32);

    match numbers {
        (.., second, ..) => {
            println!("Some numbers: {}", second)
        },
    }
}
error: `..` can only be used once per tuple or tuple struct pattern
 --> src/main.rs:5:22
  |
5 |         (.., second, ..) => {
  |                      ^^
```

Rust 不可能决定在元组中匹配 `second` 值之前应该忽略多少个值，以及在之后忽略多少个值。这段代码可能表明我们意在忽略 `2`，绑定 `second` 为 `4`，接着忽略 `8`、`16` 和 `32`；抑或是意在忽略 `2` 和 `4`，绑定 `second` 为 `8`，接着忽略 `16` 和 `32`，以此类推。变量名 `second` 对于 Rust 来说并没有任何特殊意义，所以会得到编译错误，因为在这两个地方使用 `..` 是有歧义的。

## 匹配守卫提供的额外条件

**匹配守卫**（*match guard*）是一个指定于 `match` 分支模式之后的额外 `if` 条件，它也必须被满足才能选择此分支。匹配守卫用于表达比单独的模式所能允许的更为复杂的情况。

> 这个条件可以使用模式中创建的变量。

```rust
#![allow(unused)]
fn main() {
    let num = Some(4);

    match num {
        Some(x) if x < 5 => println!("less than five: {}", x),
        Some(x) => println!("{}", x),
        None => (),
    }
}
```

上例会打印出 `less than five: 4`。当 `num` 与模式中第一个分支比较时，因为 `Some(4)` 匹配 `Some(x)` 所以可以匹配。接着匹配守卫检查 `x` 值是否小于 `5`，因为 `4` 小于 `5`，所以第一个分支被选择。

相反如果 `num` 为 `Some(10)`，因为 10 不小于 5 所以第一个分支的匹配守卫为假。接着 Rust 会前往第二个分支，这会匹配因为它没有匹配守卫所以会匹配任何 `Some` 成员。

无法在模式中表达 `if x < 5` 的条件，所以匹配守卫提供了表现此逻辑的能力。

> 也可以在匹配守卫中使用 **或** 运算符 `|` 来指定多个模式，同时匹配守卫的条件会作用于所有的模式。

```rust
#![allow(unused)]
fn main() {
    let x = 4;
    let y = false;

    match x {
        4 | 5 | 6 if y => println!("yes"),
        _ => println!("no"),
    }
}
```

 `if` 条件作用于整个 `4 | 5 | 6` 模式，而不仅是最后的值 `6`。

匹配守卫与模式的优先级关系看起来像这样：

```rust
(4 | 5 | 6) if y => ...
```

而不是：

```rust
4 | 5 | (6 if y) => ...
```

## @ 绑定

*at* 运算符（`@`）允许我们在创建一个存放值的变量的同时测试其值是否匹配模式。

```rust
#![allow(unused)]
fn main() {
    enum Message {
        Hello { id: i32 },
    }

    let msg = Message::Hello { id: 5 };

    match msg {
        Message::Hello { id: id_variable @ 3..=7 } => {
            println!("Found an id in range: {}", id_variable)
        },
        Message::Hello { id: 10..=12 } => {
            println!("Found an id in another range")
        },
        Message::Hello { id } => {
            println!("Found some other id: {}", id)
        },
    }
}
```

上例会打印出 Found an id in range: 5。通过在 3..=7 之前指定 id_variable @，我们捕获了任何匹配此范围的值并同时测试其值匹配这个范围模式。

第二个分支只在模式中指定了一个范围，分支相关代码没有一个包含 id 字段实际值的变量。id 字段的值可以是 10、11 或 12，不过这个模式的代码并**不知情也不能使用 id 字段中的值**，因为没有将 id 值保存进一个变量。

最后一个分支指定了一个没有范围的变量，此时确实拥有可以用于分支代码的变量 id，因为这里使用了结构体字段简写语法。不过此分支中没有像头两个分支那样对 id 字段的值进行测试：任何值都会匹配此分支。

> 使用 `@` 可以在一个模式中同时测试和保存变量值。

## ref 和 ref mut

当被模式匹配命中的时候，未实现`Copy`的类型会被默认的move掉，因此，原owner就不再持有其所有权。但是有些时候，我们只想要从中拿到一个变量的（可变）引用，而不想将其move出作用域，怎么做呢？答：用`ref`或者`ref mut`。

```rust
let mut x = 5;
match x {
    ref mut mr => println!("mut ref :{}", mr),
}
// 当然了……在let表达式里也能用
let ref mut mrx = x;
```

