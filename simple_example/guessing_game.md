# 编写猜数字游戏

[TOC]

实现一个经典的新手编程问题：猜数字游戏。

它是这么工作的：程序将会随机生成一个 1 到 100 之间的随机整数。接着它会请玩家猜一个数并输入，然后提示猜测是大了还是小了。如果猜对了，它会打印祝贺信息并退出。

## 创建项目目录

在 *projects* 目录，使用 Cargo 新建一个项目，如下：

```shell
$ cargo new guessing_game
$ cd guessing_game
```

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

Cargo 忽略了 `0.9.0` 版本。此时你还会注意到 *Cargo.lock* 文件中发生了更改， 无非就是正在使用的 `rand` crate 版本改为 `0.8.4`。如果想要 `rand` 使用 `0.9.0` 版本或任何 `0.9.x` 系列的版本，则必须像这样更新 *Cargo.toml* 文件：

```toml
[dependencies]
rand = "0.9.0"
```

下一次运行 `cargo build` 时，Cargo 会从 registry（注册源） 更新可用的 crate，并根据你指定的新版本重新计算。

### 生成随机数

文件名：src/main.rs

```rust
use std::io;
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..101);

    println!("The secret number is: {}", secret_number);

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {}", guess);
}
```

#### 引入 rand::Rng

`Rng` 是一个 trait，它定义了随机数生成器应实现的方法，想使用这些方法的话，此 trait 必须在作用域中。

我们在中间添加两行。

- 在首行中，我们调用 `rand::thread_rng` 函数来为我们提供将要使用的特定随机数生成器：它位于当前执行线程的本地环境中，并从操作系统获取 seed。
- 然后我们调用随机数生成器的 `gen_range` 方法。该方法由我们刚才使用 `use rand::Rng` 语句引入的 `Rng` trait 定义。
- `gen_range` 方法获得一个区间表达式（range expression）作为参数，并在区间内生成一个随机数。我们在这里使用的区间表达式采用的格式为 `start..end`。它包括起始端，但排除终止端。所以我们需要指定 `1..101` 生成一个 1 到 100 之间的数字。或者我们可以传入区间 `1..=100`，这和前面的表达等价。

> 注意：你不可能凭空就知道应该 use 哪个 trait 以及该从 crate 中调用哪个方法，所以每个 crate 都有使用说明文档。Cargo 有一个很棒的功能是：运行 `cargo doc --open` 命令来构建所有本地依赖提供的文档，并在浏览器中打开。例如，假设你对 `rand` crate 中的其他功能感兴趣，你可以运行 `cargo doc --open` 并点击左侧导航栏中的 `rand`。

新添加的第二行代码打印出了秘密数字。这在开发程序时很有用，因为可以测试它，不过在最终版本中会删掉它。如果游戏一开始就打印出结果就没什么可玩的了！

尝试运行程序几次，应该能得到不同的随机数，同时它们应该都是在 1 和 100 之间的。

## 比较猜测的数字和秘密数字

文件名：src/main.rs

示例 2-4：处理比较两个数字可能的返回值

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    // --snip--

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
}
```

### 引入 Ordering

加入另一个 `use` 声明，从标准库引入一个叫做 `std::cmp::Ordering` 的类型到作用域中。`Ordering` 也是一个枚举，不过它的成员是 `Less`、`Greater` 和 `Equal`。这是比较两个值时可能出现的三种结果。

### cmp

`cmp` 方法用来比较两个值并可以在**任何可比较的值**上调用。

它获取一个被比较值的引用：这里是把 `guess` 与 `secret_number` 做比较。 然后它会返回一个刚才通过 `use` 引入作用域的 `Ordering` 枚举的成员。

### match

使用一个 [`match`](https://rustwiki.org/zh-CN/book/ch06-02-match.html) 表达式，根据对 `guess` 和 `secret_number` 调用 `cmp` 返回的 `Ordering` 成员来决定接下来做什么。

> 一个 `match` 表达式由**分支（arm）** 构成。一个分支包含一个用于匹配的**模式**（*pattern*）。
>
> 给到 `match` 的值与分支模式相匹配时，应该执行对应分支的代码。

假设用户猜了 50，这时随机生成的秘密数字是 38。比较 50 与 38 时，因为 50 比 38 要大，`cmp` 方法会返回 `Ordering::Greater`。`Ordering::Greater` 是 `match` 表达式得到的值。它检查第一个分支的模式，`Ordering::Less` 与 `Ordering::Greater`并不匹配，所以它忽略了这个分支的代码并来到下一个分支。下一个分支的模式是 `Ordering::Greater`，**正确**匹配 `Ordering::Greater`！这个分支关联的代码被执行，在屏幕打印出 `Too big!`。`match` 表达式就此终止，因为该场景下没有检查最后一个分支的必要。

### 不匹配的类型

示例 2-4 的代码并不能编译，可以尝试一下：

```shell
$ cargo build
   Compiling libc v0.2.86
   Compiling getrandom v0.2.2
   Compiling cfg-if v1.0.0
   Compiling ppv-lite86 v0.2.10
   Compiling rand_core v0.6.2
   Compiling rand_chacha v0.3.0
   Compiling rand v0.8.3
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
error[E0308]: mismatched types
  --> src/main.rs:22:21
   |
22 |     match guess.cmp(&secret_number) {
   |                     ^^^^^^^^^^^^^^ expected struct `String`, found integer
   |
   = note: expected reference `&String`
              found reference `&{integer}`

error[E0283]: type annotations needed for `{integer}`
   --> src/main.rs:8:44
    |
8   |     let secret_number = rand::thread_rng().gen_range(1..101);
    |         -------------                      ^^^^^^^^^ cannot infer type for type `{integer}`
    |         |
    |         consider giving `secret_number` a type
    |
    = note: multiple `impl`s satisfying `{integer}: SampleUniform` found in the `rand` crate:
            - impl SampleUniform for i128;
            - impl SampleUniform for i16;
            - impl SampleUniform for i32;
            - impl SampleUniform for i64;
            and 8 more
note: required by a bound in `gen_range`
   --> /Users/carolnichols/.cargo/registry/src/github.com-1ecc6299db9ec823/rand-0.8.3/src/rng.rs:129:12
    |
129 |         T: SampleUniform,
    |            ^^^^^^^^^^^^^ required by this bound in `gen_range`
help: consider specifying the type arguments in the function call
    |
8   |     let secret_number = rand::thread_rng().gen_range::<T, R>(1..101);
    |                                                     ++++++++

Some errors have detailed explanations: E0283, E0308.
For more information about an error, try `rustc --explain E0283`.
error: could not compile `guessing_game` due to 2 previous errors

```

错误的核心表明这里有**不匹配的类型**（*mismatched type*）。

Rust 有一个**静态强类型**系统，同时也有**类型推断**。

> 当我们写出 `let guess = String::new()` 时，Rust 推断出 `guess` 应该是 `String` 类型，并不需要我们写出类型。
>
> `secret_number` 是数字类型。Rust 中有好几种数字类型拥有 1 到 100 之间的值：32 位数字 `i32`、32 位无符号数字 `u32`、64 位数字 `i64`，等等。Rust 默认使用 `i32`，这是 `secret_number` 的类型，除非额外指定类型信息，或任何能让 Rust 推断出不同数值类型的信息。

这里错误的原因在于 Rust 不会比较字符串类型和数字类型。

所以我们必须把从输入中读取到的 `String` 转换为一个真正的数字类型，才好与秘密数字进行比较。

### 类型转换

```rust
    // --snip--

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    let guess: u32 = guess.trim().parse().expect("Please type a number!");

    println!("You guessed: {}", guess);

    match guess.cmp(&secret_number) {
        Ordering::Less => println!("Too small!"),
        Ordering::Greater => println!("Too big!"),
        Ordering::Equal => println!("You win!"),
    }
```

类型转换的代码：

```rust
let guess: u32 = guess.trim().parse().expect("Please type a number!");
```

#### 遮蔽

这里创建了一个叫做 `guess` 的变量。

> Rust 允许用一个新值来**遮蔽** （*shadow*） `guess` 之前的值。这允许我们复用 `guess` 变量的名字，而不是被迫创建两个不同变量，诸如 `guess_str` 和 `guess` 之类。

#### trim()

我们将这个新变量绑定到 `guess.trim().parse()` 表达式上。表达式中的 `guess` 是指原始的 `guess` 变量，其中包含作为字符串的输入。

> `String` 实例的 `trim` 方法会去除字符串开头和结尾的空白字符，我们必须执行此方法才能将字符串与 `u32` 比较，因为 `u32` 只能包含数值型数据。
>
> 用户必须输入 enter 键才能让 `read_line` 返回，并输入他们的猜想，这会在字符串中增加一个换行符。例如，用户输入 5 并按下 enter，`guess` 看起来像这样：`5\n`，`\n` 代表 “换行”（在 Windows 中，按 enter 键会得到一个回车和一个换行符 `\r\n`）。`trim` 方法会消除 `\n` 或 `\r\n`，只留下 `5`。

#### parse()

[字符串的 `parse` 方法](https://rustwiki.org/zh-CN/std/primitive.str.html#method.parse) 将字符串解析成数字。因为这个方法可以解析多种数字类型，因此需要告诉 Rust 具体的数字类型，这里通过 `let guess: u32` 指定。`guess` 后面的冒号（`:`）告诉 Rust 我们指定了变量的类型。Rust 有一些内建的数字类型；`u32` 是一个无符号的 32 位整型。对于不大的正整数来说，它是不错的类型。另外，程序中的 `u32` 标注以及与 `secret_number` 的比较，意味着 Rust 会推断出 `secret_number` 也是 `u32` 类型。

#### 潜在错误处理

由于 `parse` 方法只能用于可以逻辑转换为数字的字符，所以调用它很容易产生错误。例如，字符串中包含 `A👍%`，就无法将其转换为一个数字。因此，`parse` 方法返回一个 `Result` 类型。像前面 [“使用 `Result` 类型来处理潜在的错误”](https://rustwiki.org/zh-CN/book/ch02-00-guessing-game-tutorial.html#使用-result-类型来处理潜在的错误) 部分讨论的 `read_line` 方法那样，再次按部就班地用 `expect` 方法处理即可。如果 `parse` 不能从字符串生成一个数字，返回一个 `Result` 的 `Err` 成员时，`expect` 会使游戏崩溃并打印附带的信息。如果 `parse` 成功地将字符串转换为一个数字，它会返回 `Result` 的 `Ok` 成员，然后 `expect` 会返回 `Ok` 值中的数字。

#### 正确运行

```shell
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 0.43s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 58
Please input your guess.
  76
You guessed: 76
Too big!
```

## 使用循环来允许多次猜测

文件名：src/main.rs

```rust
    // --snip--

    println!("The secret number is: {}", secret_number);

    loop {
        println!("Please input your guess.");

        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => println!("You win!"),
        }
    }
}
```

### loop

`loop` 关键字创建了一个**无限循环**。我们增加循环来给用户更多猜数字的机会。

### 意外退出

用户总能使用 ctrl-c 终止程序。不过还有另一个方法跳出无限循环，就是[“比较猜测的数字和秘密数字”](https://rustwiki.org/zh-CN/book/ch02-00-guessing-game-tutorial.html#比较猜测的数字和秘密数字)部分提到的 `parse`：如果用户输入的答案不是一个数字，程序会崩溃。我们可以利用这一点来退出，如下所示：

```shell
$ cargo run
   Compiling guessing_game v0.1.0 (file:///projects/guessing_game)
    Finished dev [unoptimized + debuginfo] target(s) in 1.50s
     Running `target/debug/guessing_game`
Guess the number!
The secret number is: 59
Please input your guess.
45
You guessed: 45
Too small!
Please input your guess.
60
You guessed: 60
Too big!
Please input your guess.
59
You guessed: 59
You win!
Please input your guess.
quit
thread 'main' panicked at 'Please type a number!: ParseIntError { kind: InvalidDigit }', src/main.rs:28:47
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

输入 `quit` 将会退出程序，同时你会注意到输入其他任何非数字也一样。然而，这并不理想，我们想要当猜测正确的数字时游戏也能自动退出。

### 猜测正确后退出

文件名：src/main.rs

```rust
        // --snip--

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

通过在 `You win!` 之后增加一行 `break`，用户猜对了神秘数字后会退出循环。退出循环也意味着退出程序，因为循环是 `main` 的最后一部分。

### 处理无效输入

文件名：src/main.rs

```rust
        // --snip--

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        // --snip--
```

我们将 `expect` 调用换成 `match` 语句，从而实现遇到错误就崩溃转换成处理错误。须知 `parse` 返回一个 `Result` 类型，而 `Result` 是一个拥有 `Ok` 或 `Err` 成员的枚举。这里使用的 `match` 表达式，和之前处理 `cmp` 方法返回 `Ordering` 时用的一样。

如果 `parse` 能够成功的将字符串转换为一个数字，它会返回一个包含结果数字的 `Ok`。这个 `Ok` 值与 `match` 第一个分支的模式相匹配，该分支对应的动作返回 `Ok` 值中的数字 `num`，最后如愿变成新创建的 `guess` 变量。

如果 `parse` **不**能将字符串转换为一个数字，它会返回一个包含更多错误信息的 `Err`。`Err` 值不能匹配第一个 `match` 分支的 `Ok(num)` 模式，但是会匹配第二个分支的 `Err(_)` 模式：`_` 是一个通配符值，本例中用来匹配所有 `Err` 值，不管其中有何种信息。所以程序会执行第二个分支的动作，`continue` 意味着进入 `loop` 的下一次循环，请求另一个猜测。这样程序就有效的忽略了 `parse` 可能遇到的所有错误！

## 完整代码

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..101);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}
```

