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

```rust
fn main() {
    println!("Hello, world!");
}
```

