# 解引用（*）

## 概述

引用是对内存块的借用，Rust里每一个内存块都是有主人的，主人就是对内存拥有所有权的变量，没有主人的内存块我们称之为内存泄露了。

解引用是通过引用找到内存块真正的主人，然后你可以跟主人借一些不同类型的引用，比如从&mut T借成Pin<&mut T>。

这个主人可能被包了很多层，当你以为你找了主人，其实它只是个皮，所以会存在不断解引用的情况，你可能需要加很多个*，当然很多个*不符合设计美感。

Rust语法规定，同一块内存只能有一个可变引用，或者有多个不可变引用。

Rust之所以这么规定，一个非常大的优点是避免了内存被多处修改的潜在隐患，避免了资源的复杂环境竞争，降低了程序的调试难度。

那么，程序编写过程中必然会在不同的函数块里调用同一块内存，所以引用的使用将会变得非常频繁，我们犯的错误大多也在此。

## 解引用的方法

解引用可以分为，自动解引用和手动解引用。

Rust为了减少某些场合下重复解引用导致的代码美观问题，在编译期做了一些智能识别功能，比如带有&T参数的函数被调用的时候，你传&&&......&&&T都可以自动解引用，直到符合函数的参数类型为止。

手动解引用，就是和其他语言类似，借用是&操作符，解引用是*操作符。

我们也可以通过自行实现Deref这个Trait来自定义解引用的最终目标是什么，而恰恰这个也是Rust语言最难的地方，你得了解每个类型是否实现了Deref，而Rust类型实在是太多了，连&T借用也算一个新的类型，&T是不能继承T的所有特性的。

## 总结

`*`操作符的特性是，先将作用对象进行deref得到一个引用，再将&引用清除。如果只是想得到一个引用，那么使用`&*`或者`deref（）`。

如果不好理解，可以这么去记忆：`*`是用来消除`&`和`box`符号的，当发现目标没有`&`和`box`，就会调用`deref`函数去生成带有&符号的类型，然后就可以愉快的消除了。
