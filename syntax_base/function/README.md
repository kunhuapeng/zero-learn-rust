# 函数（function）

## 概述

函数是一组一起执行一个任务的语句块。

函数是一段可读，可维护和可重用代码的多条语句。

每个 Rust 程序都至少有一个函数，即主函数 `main()`。

除了使用 Rust 核心和标准库提供的函数外，我们还可以自己定义自己的函数。

| 术语       | 说明                                           |
| ---------- | ---------------------------------------------- |
| 函数定义   | 函数定义其实就是定义一个任务要以什么方式来完成 |
| 函数调用   | 函数只有被调用才会运行起来                     |
| 函数返回值 | 函数在执行完成后可以返回一个值给它的调用者     |
| 函数参数   | 函数参数用于携带外部的值给函数内部的代码       |

## 函数定义

### 命名规则

函数和变量名使用下划线命名法（*snake case*，直译为蛇形命名法）规范风格。

在下划线命名法中，所有字母都是小写并使用下划线分隔单词。

例如：

```rust
fn main() {
    println!("Hello, world!");

    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

### 定义函数的语法

定义函数时必须使用 `fn` 关键字开头，后跟着函数名和一对圆括号，圆括号内可以定义函数的参数；

如果 函数返回一个值，返回类型必须在箭头 `->` 之后特别指出来。

> 定义函数时必须以 `fn` 关键字开头，`fn` 关键字是 `function` 的缩写。

> Rust 不关心函数定义于何处，只要定义了就行。

```rust
fn function_name([param1:data_type1,param2..paramN]) -> return_type {
   // 函数代码
}
```

范例：定义一个简单的函数

```rust
// 函数定义
fn fn_hello(){
   println!("hello from function fn_hello ");
}
```

## 函数调用

让函数运行起来的过程我们称之为 **函数调用**。

如果函数定义了参数，那么在 **函数调用** 时必须传递指定类型的参数。

在一个函数 `fn1` 内部调用其它函数 `fn2`，那么这个 `fn1` 函数就称为 **调用者函数**，简称为 **调用者**。

### 函数调用的语法

```rust
function_name(val1,val2,valN)
```

例如我们在 `main()` 函数内部调用函数 `fn_hello()`

```rust
fn main(){
   //调用函数
   fn_hello();
}
```

这时候，函数 `main()` 就是 **调用者函数**，也就是 **调用者**。

**范例**

```rust
fn main(){
   // 调用函数
   fn_hello();
}

// 定义函数
fn fn_hello(){
   println!("hello from function fn_hello ");
}
```

## 函数返回值

函数可以返回值给它的调用者。我们将这些值称为 **函数返回值**。

也就是说，函数在代码执行完成后，除了将控制权还给调用者之外，还可以携带值给它的调用者。

如果一个函数需要返回值给它的调用者，那么我们在函数定义时就需要明确中函数能够返回什么类型的值。

### 函数返回值语法

返回值是通过在 **小括号后面使用 箭头 ( `->` ) 加上数据类型** 来定义的。

> 在函数的代码中，可以使用 `return` 关键字指定要返回的值。

> 如果函数代码中没有使用 `return` 关键字，那么函数会默认使用最后一条语句的执行结果作为返回值。

> `return` 中返回的值或最后一条语句的执行结果 必须和函数定义时的返回数据类型一样，不然会编译会出错

####  有 `return` 语句

```rust
function function_name() -> return_type {
   // 其它代码

   // 返回一个值
   return value;
}
```

####  没有 `return` 语句

最后一条语句的结果作为返回值

函数中最后用于返回值的语句不能有 **分号 `;`** 结尾，否则就不会时返回值了。

```rust
function function_name() -> return_type {
   // 其它代码

   value // 没有分号则表示返回值
}
```

**范例**

我们定义了两个相同功能的`get_pi()` 和 `get_pi2()` 函数，一个使用 `return` 语句返回值，另一个则使用没有分号的最后一条语句作为返回值。

```rust
fn main(){
   println!("pi value is {}",get_pi());
   println!("pi value is {}",get_pi2());
}

fn get_pi()->f64 {
   22.0/7.0
}

fn get_pi2()->f64 {
   return 22.0/7.0;
}
```

> 如果函数定义了返回值，而且未使用`return`，函数的返回值必须 **没有分号** 结尾。

### 接收返回值

我们可以将函数的返回值赋值给一个变量。

```rust
fn main(){
   let pi = get_pi();
   println!("pi value is {}",pi);
}

fn get_pi()->f64 {
   22.0/7.0
}
```

## 函数参数

**函数参数** 是一种将外部变量和值带给函数内部代码的一种机制。函数参数是函数签名的一部分。

当一个函数定义了函数参数，那么在调用该函数的之后就可以把变量/值传递给函数。

我们把函数定义时指定的参数名叫做 **形参**。同时把调用函数时传递给函数的变量/值叫做 **实参**。

> 除非特别指定，函数调用时传递的 **实参** 数量和类型必须与 **形参** 数量和类型必须相同。 否则会编译错误。

函数参数有两种传值方法，一种是把值直接传递给函数，另一种是把值在内存上的保存位置传递给函数。

### 函数参数语法

传值调用

```rust
fn function_name([param1:data_type1,param2..paramN]) {
   // 函数代码
}
```

引用调用，在参数类型前加`&`

```rust
fn function_name([param1:&data_type1,param2..paramN]) {
   // 函数代码
}
```

### 传值调用

**传值调用** 就是简单的把传递的变量的值传递给函数的 **形参**，从某些方面说了，就是把函数参数也赋值为传递的值。

因为是赋值，所以函数参数和传递的变量其实是各自保存了相同的值，**互不影响**。因此函数内部修改函数参数的值并不会影响外部变量的值。

这是因为传值调用传递的是变量的一个副本，也就是**重新创建**了一个变量。

> 确切地说，只有存在**栈**中的数据，会出现传值调用的情况。拷贝一份新的数据传给函数。
>
> 如果参数的数据是存在**堆**中复合类型数据，则会出现**所有权转移**的情况。一旦函数运行完成，就会自动清除，导致数据被清除。想避免这种情况，可以使用**引用调用**，或者**作为返回值**返回。

**范例**

```rust
fn main(){
   let no:i32 = 5;
   mutate_no_to_zero(no);
   println!("The value of no is:{}",no);
}

fn mutate_no_to_zero(mut param_no: i32) {
   param_no = param_no*0;
   println!("param_no value is :{}",param_no);
}
```

运行结果：

```shell
param_no value is :0
The value of no is:5
```

### 引用调用

值传递变量导致重新创建一个变量。但引用传递则不会，引用传递把当前变量的内存位置传递给函数。

对于引用传递来说，传递的变量和函数参数都共同指向了同一个内存位置。

引用传递需要函数定义时在参数类型的前面加上 `&` 符号，语法格式如下

```rust
fn function_name(parameter: &data_type) {
   // 函数的具体代码
}
```

**范例**

```rust
fn main() {
   let mut no:i32 = 5;
   mutate_no_to_zero(&mut no);
   println!("The value of no is:{}",no);
}
fn mutate_no_to_zero(param_no:&mut i32){
   *param_no = 0; //解引用操作
}
```

运行结果

```shell
The value of no is 0.
```

> 上面的代码中，**星号（`*`）** 用于访问变量 `param_no` 指向的内存位置上存储的变量的值。这种操作也称为 **解引用**。 因此 **星号（`*`）** 也称为 **解引用操作符**。

### 复合类型参数

对于复合类型，比如字符串，如果按照普通的方法传递给函数后，那么该变量将不可再访问

这涉及到**堆栈**、**所有权**、**所有权转移**、**作用域** 和 **引用**等。

下面的代码编译会报错

```rust
fn main(){
   let name:String = String::from("TutorialsPoint");
   display(name); 
   println!("after function name is: {}",name);
}

fn display(param_name:String){
   println!("param_name value is :{}",param_name);
}
```

编译上面的代码会出错，错误信息如下

```shell
error[E0382]: borrow of moved value: `name`
 --> src/main.rs:4:42
  |
2 |    let name:String = String::from("TutorialsPoint");
  |        ---- move occurs because `name` has type `std::string::String`, which does not implement the `Copy` trait
3 |    display(name); 
  |            ---- value moved here
4 |    println!("after function name is: {}",name);
  |                                          ^^^^ value borrowed here after move
```

出现上述错误的原因，就是因为函数在调用的时候，变量`name`的值出现了**所有权转移**的情况。

数据的所有权转移给了函数的形参`param_name`。当函数运行完成时，会清除内容，`param_name`的数据就被清除。

而`name`已经没有数据的所有权，所以此时使用该变量，会出现错误。

#### 解决方法：引用调用

```rust
fn main(){
   let name:String = String::from("TutorialsPoint");
   display(&name); 
   println!("after function name is: {}",name);
}

fn display(param_name:&String){
   println!("param_name value is :{}",param_name);
}
```

这样就不会出错了。
