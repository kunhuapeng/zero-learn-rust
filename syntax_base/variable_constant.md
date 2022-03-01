# 变量与常量

## 变量

### 命名规范

1. > 变量名中可以包含 **字母**、**数字** 和 **下划线**。

   也就只能是 `abcdefghijklmnopqrstuvwxyz013456789_` 以及大写字母。

2. > 变量名必须以 **字母** 或 **下划线** 开头。

   也就是不能以 **数字** 开头。

3. > 变量名是 **区分大小** 写的。

   也就是大写的 `A` 和小写的 `a` 是两个不同的字符

### 声明语法 *let*

Rust 语言中声明变量的语法格式如下：

```rust
let variable_name = value;            // 不指定变量类型
let variable_name:dataType = value;   // 指定变量类型
```

范例：

```rust
fn main() {
   let fees = 25_000;
   let salary:f64 = 35_000.00;
   println!("fees is {} and salary is {}",fees,salary);
}
```

### 变量的初始化

> 变量必须先声明，后使用。
>
> 变量使用前必须初始化。
>
> 变量可以在声明的同时初始化；也可以先声明，在使用前初始化。

范例：

```rust
fn main() {
    let a:i32;
    a = 5;
    println!("{}", a);  //5
    
    let x:i32;
    println!("{}", x);  //error[E0381]: borrow of possibly uninitialized variable: `x`
}
```

Rust中，类型没有"默认构造函数"，变量没有“默认值”。对于 `let x:i32;` 如果没有显示赋值，它就没有被初始化。

### 变量的不可变性

> Rust 语言中使用 `let` 声明的变量。
>
> 默认情况下，Rust 语言中的变量是不可变的。
>
> 在第一次赋值之后，是不可变更不可重新赋值的，变成了 **只读** 状态。

范例：

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);
    x = 6;
    println!("The value of x is: {}", x);
}
```

编译上面这段 Rust 代码，是会报错的

```shell
$ cargo run
   Compiling variables v0.1.0 (file:///projects/variables)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:4:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: consider making this binding mutable: `mut x`
3 |     println!("The value of x is: {}", x);
4 |     x = 6;
  |     ^^^^^ cannot assign twice to immutable variable

For more information about this error, try `rustc --explain E0384`.
error: could not compile `variables` due to previous error
```

上面的错误指出错误的原因是 `cannot assign twice to immutable variable `x``（不能对不可变变量二次赋值），因为我们尝试给不可变的 `x` 变量赋值为第二个值。

### 可变变量 *mut*

> Rust 语言提供了 `mut` 关键字表示 **可变更**。 
>
> 我们可以在变量名的前面加上 `mut` 关键字来告诉编译器这个变量是可以重新赋值的。

#### 可更变量的声明语法

```rust
let mut variable_name:dataType = value;
```

或者让编译器自动推断类型

```rust
let mut variable_name = value;
```

范例：

```rust
fn main() {
   let mut fees:i32 = 25_000;
   println!("fees is {} ",fees);
   fees = 35_000;
   println!("fees changed is {}",fees);
}
```

运行结果：

```shell
fees is 25000
fees changed is 35000
```

### 变量遮蔽 *Shadow*ing

通过使用相同变量的名称并使用`let`关键字来复用变量。

范例：

```rust
fn main() {
    let x = 1;  //作用域：从定义到下一个x
    let  mut addr = &x as *const i32 as usize;
    println!("The value of x is:{}， adder is {:X}", x, addr);

    let x = x + 1;  //OS又重新分配了一个空间，这个空间的名字叫做x
    addr = &x as *const i32 as usize;
    println!("The value of x is:{}， adder is {:X}", x, addr);

    let x = x * 2; //新创建一个叫做x的不可变变量
    addr = &x as *const i32 as usize;
    println!("The value of x is:{}， adder is {:X}", x, addr);
}
```

运行结果：

```shell
The value of x is:1， adder is 55FB4FF66C
The value of x is:2， adder is 55FB4FF6DC
The value of x is:4， adder is 55FB4FF744
```

地址不同，说明它们是不同的变量，只是名字相同而已。

当再次使用 let 时，实际上创建了一个新变量，我们可以改变值的类型，但复用这个名字。

### *Shadowing* 与 *mut* 的区别

>*Shadowing*再次创建一个新的变量，可以改变该变量的类型。
>
>`mut`来改变类型值，将得到`mismatched types`异常。

## 常量

常量就是那些值不能被改变的变量。一旦我们定义了一个常量，那么就再也没有任何方法可以改变常量的值了。

### 命名规范

1. > 常量名中可以包含 **字母**、**数字** 和 **下划线**。但是常量名通常都是 **大写字母**。

   也就只能是 `013456789_` 以及大写字母。

2. > 常量名必须以 **字母** 或 **下划线** 开头。

   也就是不能以 **数字** 开头。

### 声明语法  *const*

Rust 语言中使用 `const` 关键字来定义一个常量。

Rust 中定义常量的语法格式如下:

```rust
const VARIABLE_NAME:dataType = value;
```

>常量声明时必须指明数据类型。
>
>常量声明时必须赋值。
>
>常量只能 被赋值为 常量表达式/数学表达式，不能是 函数返回值 或者其它只有在运行时才能确定的值。
>
>常量可以在任意作用域里定义，包括全局作用域。
>
>常量声明不可以使用`mut`关键字。
>
>常量不可以被遮蔽。

范例：

```rust
fn main() {
   const USER_LIMIT:i32 = 100;    // 定义了一个 i32 类型的常量
   const PI:f32 = 3.14;           // 定义了一个 float 类型的常量

   println!("user limit is {}",USER_LIMIT);  // 显示常量的值
   println!("pi value is {}",PI);            // 显示常量的值
}
```

