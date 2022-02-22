# 安装Rust

## 在 Windows 上安装 rustup

在 Windows 上，访问 https://www.rust-lang.org/zh-CN/tools/install 页面并按照说明安装 Rust。在安装过程的某个步骤，你可能会收到一条消息，提示你还需要适用于 Visual Studio 2013 或更高版本的 C++ 的构建工具（C++ build tools）。获取这些构建工具的最简单方法是安装 Visual Studio 2019 的构建工具。当被问及要安装哪些内容时，请确保已选择 “C++ build tools”，并包括 Windows 10 SDK 和英文语言包。

本书的其余部分使用的命令行在 cmd.exe 和 PowerShell 中都可以运行。如果有特定差异，我们将说明使用哪个。

## 常用命令

### 更新

通过 `rustup` 安装 Rust 后，更新到最新版本很简单。在 shell 中运行以下更新命令：

```shell
$ rustup update
```

### 卸载

要卸载 Rust 和 `rustup`，在 shell 中运行以下卸载命令：

```shell
$ rustup self uninstall
```

### 版本查看

要检查是否正确安装了 Rust，可打开 shell 并输入下面这行：

```shell
$ rustc --version
```

### 本地文档

Rust 的安装还自带文档的本地副本，可以方便地离线阅读。

```shell
rustup doc
```

