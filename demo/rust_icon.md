# 应用程序图标设置

给自己打包出来的可执行文件添加个图标。

## 概述

需要创建一个`.rc`文件声明图标的资源文件`.ico`,然后通过库`embed-resource`在构建时编译即可 icon图标的大小最好为 `256px*256px`

## 创建项目

```shell
cargo new my_icon
```

## 修改`cargo.toml`

在`cargo.toml`中添加对`embed-resource`的引用:

```toml
[build-dependencies]
embed-resource = "1.5.1"
```

> 注意，是加到`[build-dependencies]`块中，而不是`[dependencies]`块。

## 创建`.rc`文件

`.rc`文件的文件名随意设置，比如我们命名为`icon.rc`，放在项目的根目录，内容如下:

```
// 第一列为任意名称即可,第二列为固定ICON用于设置资源类型,第三列为ico文件的路径
iconName ICON "icon.ico"
```

> 同时，将`ico`文件也拷贝到项目根目录

## 创建`build.rs`文件

在项目根目录创建`build.rs`文件。文件内容：

```rust
extern crate embed_resource;
fn main() {
    embed_resource::compile("./icon.rc");//--路径根据实际情况设置
}
```

## 编译运行

```shell
cargo run
```

如果对应的路劲设置都是正确的,则能正常通过编译,且程序的图标也应该设置成功
可以在资源管理器中和任务栏已经进程管理器中看到程序图标的变化