# 编写猜数字游戏

[TOC]

实现一个经典的新手编程问题：猜数字游戏。

它是这么工作的：程序将会随机生成一个 1 到 100 之间的随机整数。接着它会请玩家猜一个数并输入，然后提示猜测是大了还是小了。如果猜对了，它会打印祝贺信息并退出。

----

## 创建项目目录

在 *projects* 目录，使用 Cargo 新建一个项目，如下：

```shell
$ cargo new guessing_game
$ cd guessing_game
```

----

## 处理一次猜测

提示用户输入猜测，获取用户猜测并打印出来。

文件名：src/main.rs

```rust
use std::io;//--从标准库中引入输入/输出功能

fn main() {
    println!("Guess the number!");//--提示信息

    println!("Please input your guess.");//--提示用户输入

    let mut guess = String::new();//--定义变量接收用户的输入

    //--用变量接收用户的输入
    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);//--将用户输入的猜测打印到屏幕
}
```

示例 2-1：获取用户猜测并打印的代码

### 逐行解读代码

#### 引入io

```rust
use std::io;
```

默认情况下，Rust会将少量标准库中定义的程序项（item）引入到每个程序的作用域中。这些项称作 *prelude*。

如果需要的类型不在 prelude 中，你必须使用 `use` 语句显式地将其引入作用域。

`std::io` 库提供很多有用的功能，包括接收用户输入的功能。

#### 程序入口

```rust
fn main() {
```

`main` 函数是程序的入口点。

`fn` 语法声明了一个新函数，小括号 `()` 表明没有参数，大括号 `{` 作为函数体的开始。

```rust
    println!("Guess the number!");

    println!("Please input your guess.");
```

这些代码仅仅打印提示，介绍游戏的内容然后请求用户输入。

#### 定义变量

```rust
    let mut guess = String::new();
```

创建一个储存用户输入的**变量**（*variable*）

> 我们使用 `let` 声明来创建变量。
>
> 在 Rust 中，变量默认是**不可变**的。
>
> 在变量名前使用 `mut` 来使一个变量可变。

例如：

```rust
let apples = 5; // 不可变
let mut bananas = 5; // 可变
```

> 注意：`//` 语法开始一个注释，持续到行尾。Rust 忽略注释中的所有内容，[第 3 章](https://rustwiki.org/zh-CN/book/ch03-04-comments.html)将会详细介绍注释。

现在我们知道了 `let mut guess` 会引入一个叫做 `guess` 的可变变量。等号（`=`）告诉 Rust 现在想将某个值绑定在变量上。等号的右边是 `guess` 所绑定的值，它是 `String::new` 的结果，这个函数会返回一个 `String` 的新实例。`String` 是一个标准库提供的字符串类型，这是 UTF-8 编码的可增长文本块。

> `::new` 那一行的 `::` 语法表明 `new` 是 `String` 类型的一个关联函数。
>
> **关联函数**（*associated function*）是实现一种特定类型的函数，在这个例子中类型是 `String`。
>
> 这个 `new` 函数创建了一个新的空字符串，你会发现很多类型上有 `new` 函数，因为它是创建类型实例的惯用函数名。

总的来说，`let mut guess = String::new();` 这一行创建了一个可变变量，当前它绑定到一个新的 `String` 空实例上。

#### 读取用户输入

```rust
    io::stdin()
        .read_line(&mut guess)
```

我们在程序的第一行使用 `use std::io;` 从标准库中引入了输入/输出功能。现在调用可以使我们处理用户输入的 `io` 库中的函数 `stdin`。

如果程序的开头没有使用 `use std::io` 引入 `io` 库，我们仍可以通过把函数调用写成 `std::io::stdin` 来使用函数。`stdin` 函数返回一个 [`std::io::Stdin`](https://rustwiki.org/zh-CN/std/io/struct.Stdin.html) 的实例，这代表终端标准输入句柄的类型。

接下来，`.read_line(&mut guess)` 这一行调用了 [`read_line`](https://rustwiki.org/zh-CN/std/io/struct.Stdin.html#method.read_line) 方法从标准输入句柄获取用户输入。我们还将 `&mut guess` 作为参数传递给 `read_line()`，以告诉它在哪个字符串存储用户输入。`read_line` 的全部工作是，无论用户在标准输入中键入什么内容，都将其存入一个字符串中（不覆盖其内容），所以它需要字符串作为参数。这个字符串参数需要是可变的，以便该方法可以更改字符串的内容。

> `&` 表示这个参数是一个**引用**（*reference*），它允许多处代码访问同一处数据，而无需在内存中多次拷贝。
>
> 引用是一个复杂的特性，Rust 的一个主要优势就是安全而简单的操作引用。
>
> 就像变量一样，引用默认是不可变的。因此，需要写成 `&mut guess` 来使其可变，而不是 `&guess`。
>

#### 处理潜在错误

```rust
        .expect("Failed to read line");
```

使用 Result 类型来处理潜在的错误

 `read_line` 将用户输入放置到传递给它的字符串中，不过它也返回一个值——在这个例子中是 [`io::Result`](https://rustwiki.org/zh-CN/std/io/type.Result.html)。Rust 标准库中有很多叫做 `Result` 的类型：一个通用的 [`Result`](https://rustwiki.org/zh-CN/std/result/enum.Result.html) 以及在子模块中的特化版本，比如 `io::Result`。`Result` 类型是 [*枚举*（*enumerations*）](https://rustwiki.org/zh-CN/book/ch06-00-enums.html)，通常也写作 *enum*。枚举类型持有固定集合的值，这些值被称为枚举的**成员**（*variant*）。枚举往往与条件表达式 `match` 一起使用，可以方便地根据枚举值是哪个成员来执行不同的代码。

> `Result` 的成员是 `Ok` 和 `Err`。
>
> `Ok` 成员表示操作成功，内部包含成功时产生的值。
>
> `Err` 成员则意味着操作失败，并且包含失败的前因后果。

这些 `Result` 类型的作用是编码错误处理信息。`Result` 类型的值，像其他类型一样，拥有定义于其上的方法。`io::Result` 的实例拥有 [`expect` 方法](https://rustwiki.org/zh-CN/std/result/enum.Result.html#method.expect)。如果 `io::Result` 实例的值是 `Err`，`expect` 会导致程序崩溃，并显示当做参数传递给 `expect` 的信息。

> 如果 `read_line` 方法返回 `Err`，则可能是来源于底层操作系统错误的结果。
>
> 如果 `io::Result` 实例的值是 `Ok`，`expect` 会获取 `Ok` 中的值并原样返回。在本例中，这个值是用户输入的字节数。

如果不调用 `expect`，程序也能编译，不过会出现一个警告：

```shell
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
warning: unused `Result` that must be used
  --> src/main.rs:10:5
   |
10 |     io::stdin().read_line(&mut guess);
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = note: `#[warn(unused_must_use)]` on by default
   = note: this `Result` may be an `Err` variant, which should be handled

warning: `guessing_game` (bin "guessing_game") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 0.59s

```

Rust 警告我们没有使用 `read_line` 的返回值 `Result`，说明有一个可能的错误没有处理。

消除警告的正确做法是实际编写错误处理代码，不过由于我们就是希望程序在出现问题时立即崩溃，所以直接使用 `expect`。

#### 打印用户输入字符

```rust
    println!("You guessed: {}", guess);
```

打印了存储用户输入的字符串。

> 里面的 `{}` 是预留在特定位置的占位符：
>
> 把 `{}` 想象成小蟹钳，可以夹住合适的值。使用 `{}` 也可以打印多个值：
>
> 第一对 `{}` 使用格式化字符串之后的第一个值，第二对则使用第二个值，依此类推。

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
```

这行代码会打印出 `x = 5 and y = 10`。

----

## 生成一个随机数字

Rust 标准库中尚未包含随机数功能。但是，Rust 团队还是提供了一个包含上述功能的 [`rand` crate](https://crates.io/crates/rand)。

### 使用 crate 来增加更多功能

> *crate* 是一个 Rust 代码包。
>
> 我们正在构建的项目是一个 **二进制 crate**，它生成一个可执行文件。 
>
> `rand` crate 是一个 **库 crate**，库 crate 可以包含任意能被其他程序使用的代码，但是不能独自执行。

Cargo 对外部 crate 的运用是其真正的亮点所在。在我们使用 `rand` 编写代码之前，需要修改 *Cargo.toml* 文件，引入一个 `rand` 依赖。现在打开这个文件并将下面这一行添加到 `[dependencies]` 表块标题之下。请确保按照我们这里的方式指定 `rand` 及其这里给出的版本号，否则本教程中的示例代码可能无法工作。

#### 修改Cargo.toml

文件名：Cargo.toml

```toml
rand = "0.8.3"
```

> 在 *Cargo.toml* 文件中，表头以及之后的内容属同一个表块，直到遇到下一个表头才开始新的表块。
>
> 在 `[dependencies]` 表块中，你要告诉 Cargo 本项目依赖了哪些外部 crate 及其版本。

> 本例中，我们使用语义化版本 `0.8.3` 来指定 `rand` crate。
>
> Cargo 理解[语义化版本](http://semver.org/)（Semantic Versioning，有时也称为 *SemVer*），这是一种定义版本号的标准。
>
> `0.8.3` 实际上是 `^0.8.3` 的简写，它表示任何至少包含 `0.8.3` 但低于 `0.9.0` 的版本。 Cargo 认为这些版本具有与 `0.8.3` 版本兼容的公有 API， 并且此规范可确保你将获得最新的补丁版本，它仍然可以与本章中的代码正常编译。`0.9.0` 或更高版本则不再确保 API 和以下示例所使用的 API 相同。

#### 构建项目

现在，不修改任何代码就可以构建项目

```shell
$ cargo build
    Updating crates.io index
  Downloaded rand v0.8.3
  Downloaded libc v0.2.86
  Downloaded getrandom v0.2.2
  Downloaded cfg-if v1.0.0
  Downloaded ppv-lite86 v0.2.10
  Downloaded rand_chacha v0.3.0
  Downloaded rand_core v0.6.2
   Compiling rand_core v0.6.2
   Compiling libc v0.2.86
   Compiling getrandom v0.2.2
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.10
   Compiling rand_chacha v0.3.0
   Compiling rand v0.8.3
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53s

```

可能会出现不同的版本号（多亏了语义化版本，它们与代码是兼容的！），同时显示顺序也可能会有所不同。

当我们引入了一个外部依赖后，Cargo 将从 *registry* 上获取所有依赖所需的最新版本，这是一份来自 [Crates.io](https://crates.io/) 的数据拷贝。Crates.io 是 Rust 生态环境中开发者们向他人贡献 Rust 开源项目的地方。

在更新完 registry 后，Cargo 检查 `[dependencies]` 表块并下载缺失的 crate 。本例中，虽然只声明了 `rand` 一个依赖，然而 Cargo 还是额外获取了 `rand` 所需的其他 crate，`rand` 依赖它们来正常工作。下载完成后，Rust 编译依赖，然后使用这些依赖编译项目。

如果不做任何修改，立刻再次运行 `cargo build`，则不会看到任何除了 `Finished` 行之外的输出。Cargo 知道它已经下载并编译了依赖，同时 *Cargo.toml* 文件也没有变动。Cargo 还知道代码也没有任何修改，所以它不会重新编译代码。因为无事可做，它简单的退出了。

#### 修改main.rs 文件后再次构建

如果打开 *src/main.rs* 文件，做一些无关紧要的修改，保存并再次构建，则会出现两行输出：

```shell
$ cargo build
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 2.53 secs
```

这一行表示 Cargo 只针对 *src/main.rs* 文件的微小修改而更新构建。依赖没有变化，所以 Cargo 知道它可以复用已经为此下载并编译的代码。

#### Cargo.lock 文件确保构建是可重现的

Cargo 有一个机制来确保任何人在任何时候重新构建代码，都会产生相同的结果：Cargo 只会使用你指定的依赖版本，除非你又手动指定了别的。例如，如果下周 `rand` crate 的 `0.8.4` 版本出来了，它修复了一个重要的 bug，同时也含有一个会破坏代码运行的缺陷。为了处理这个问题，Rust 在你第一次运行 `cargo build` 时建立了 *Cargo.lock* 文件，我们现在可以在 *guessing_game* 目录找到它。

当第一次构建项目时，Cargo 计算出所有符合要求的依赖版本并写入 *Cargo.lock* 文件。当将来构建项目时，Cargo 会发现 *Cargo.lock* 已存在并使用其中指定的版本，而不是再次计算所有的版本。这使得你拥有了一个自动化的可重现的构建。换句话说，项目会持续使用 `0.8.3` 直到你显式地升级，多亏有了 *Cargo.lock* 文件。

#### 更新 crate 到一个新版本

当你**确实**需要升级 crate 时，Cargo 提供了这样一个命令 `update`，它会忽略 *Cargo.lock* 文件，并计算出所有符合 *Cargo.toml* 声明的最新版本。Cargo 接下来会把这些版本写入 *Cargo.lock* 文件。不过，Cargo 默认只会寻找大于或等于 `0.8.3` 而小于 `0.9.0` 的版本。如果 `rand` crate 发布了两个新版本，`0.8.4` 和 `0.9.0`，在运行 `cargo update` 时会出现如下内容：

```shell
cargo update
    Updating crates.io index
    Updating rand v0.8.3 -> v0.8.4
```

