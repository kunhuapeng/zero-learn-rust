# Hello, Cargo!

Cargo 是 Rust 的构建系统和包管理器。大多数 Rustacean 们使用 Cargo 来管理他们的 Rust 项目，因为它可以为你处理很多任务，比如构建代码、下载依赖库，以及编译这些库。（我们把代码所需要的库叫做**依赖**（*dependency*））。

由于绝大多数 Rust 项目使用 Cargo，可以在终端输入如下命令检查是否安装了 Cargo：

```shell
$ cargo --version
```

如果你看到了版本号，说明安装成功！如果看到类似 `command not found` 的错误，你应该查看相应安装文档以确定如何单独安装 Cargo。

