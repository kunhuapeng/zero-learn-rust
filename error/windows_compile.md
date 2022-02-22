# windows下编译问题

## error: linker `link.exe` not found

### 场景

编译时报错：

```shell
rustc main.rs
```

出现：error: linker `link.exe` not found  的错误

### 解决

安装vs2019，并且安装时需要一定要选择C++的桌面开发

![image-20220221180900203](../static/images/image-20220221180900203.png)

附件：<a href="static/download/Visual_Studio_2019_Community.zip" target="_blank">Visual Studio 2019 Community_16.4.29519.181</a>

