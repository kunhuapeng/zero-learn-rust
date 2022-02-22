# Hello, World!

编写第一个 Rust 程序。

------

### 创建项目目录

打开终端，输入下面命令来创建 *projects* 目录，以在此目录里面创建 “Hello, world!” 项目目录。

对于 Windows CMD，输入以下内容：

```shell
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

项目目录，以自己的目录自行决定，不一定照搬上述命令。

或者手动在Windows窗口中创建。

------

### 编写和运行 Rust 程序

**扩展名：**Rust 文件通常以 *.rs* 扩展名结尾。

**命名规则：**如果文件名中使用了多个单词，请使用下划线将它们隔开。例如，命名为 *hello_world.rs*，而不是 *helloworld.rs*。

**示例1-1：**一个打印 `Hello, world!` 的程序

文件名：main.rs

```rust
fn main() {
    println!("Hello, world!");
}
```

保存文件， 在Windows CMD上，输入以下命令，编译并运行文件：

```shell
> rustc main.rs
> .\main.exe
Hello, world!
```

字符串 `Hello, world!` 都应打印到了终端上。

------

### Rust 程序剖析

让我们详细回顾一下 “Hello, world!” 程序发生了什么。这是拼图的第一块:

```rust
fn main() {

}
```

**`main` 函数**（也称为**主函数**）：很特殊，它始终是每个可执行 Rust 程序中运行的第一个代码。不带参数也没有返回值。

函数主体用大括号 `{}` 括起来。Rust 需要函数体的所有内容都被括号包围起来。

**代码风格：**将左大括号放在函数声明的同一行，且之间带有一个空格。如果想在 Rust 项目中坚持标准代码风格，则可以使用自动格式化程序工具 `rustfmt` 来将代码格式化为特定风格。

`main` 函数内部是以下代码：

```rust
    println!("Hello, world!");
```

该行完成了此简单程序中的所有工作：它将文本打印到屏幕上。这里有 4 个要注意的重要细节。

1.  Rust 风格的缩进使用 4 个空格，而不是制表符。
2. `println!` 调用 Rust 宏。如果改为调用函数，则应该将其输入为 `println`（不含 `!`）。当看到一个 `!`，则意味着调用的是宏而不是普通的函数。
3.  你看到 `"Hello, world!"` 字符串。我们将这个字符串作为参数传递给 `println!`，接着 `println!` 将字符串打印到屏幕上。
4.  我们用分号（`;`，注意这是英文分号）结束该行，这表明该表达式已结束，下一个表达式已准备好开始。Rust 代码的大多数行都以一个 `;` 结尾。

------

### 编译和运行是独立的步骤

在运行 Rust 程序之前，必须使用 Rust 编译器来编译它，输入 `rustc` 命令并传入源文件的名称

```shell
$ rustc main.rs
```

编译成功后，Rust 就会输出一个二进制可执行文件。

在 Windows 的 CMD，可输入以下内容：

```shell
> dir /B %= the /B option says to only show the file names =%
main.exe
main.pdb
main.rs
```

这显示了带有 *.rs* 扩展名的**源代码文件**，**可执行文件**（在 Windows 上是 *main.exe*，在所有其他平台上是 *main*），以及在使用 Windows 时包含一个带有 *.pdb* 扩展名的**调试信息**的文件。

运行 *main* 或 *main.exe* 文件：

```shell
.\main.exe
Hello, world!
```



Rust 是一门**预编译**(*ahead-of-time compiled*)语言，这意味着你可以编译一个程序，将编译后的可执行文件给别人，即使他们没有安装 Rust 也可以运行程序。

使用 `rustc` 编译对简单的程序可以轻松胜任，但随着项目的增长，你将会想要管理项目中所有相关内容，并想让其他用户和项目能够容易共享你的代码。我们将引入 Cargo 工具，这将帮助你学会编写真实开发环境的 Rust 程序。
