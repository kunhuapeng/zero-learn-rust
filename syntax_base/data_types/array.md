# 数组（array）

## 数组

数组是一组包含相同数据类型 `T` 的组合，并存储在连续的内存区中。

数组使用中括号 `[]` 来创建， 另外它们的大小在编译期间就已确定。

> 数组的类型标记为 `[T; size]`（译注：T 为元素的类型，size 表示数组的大小）。

## 特性

- 数组的定义其实就是为分配一段 **连续的相同数据类型** 的内存块。

- 数组是静态的。这意味着一旦定义和初始化，则永远**不可更改**它的长度。

- 数组的元素有着相同的数据类型，每一个元素都独占者数据类型大小的内存块。
  也就是说。数组的内存大小等于数组的长度乘以数组的数据类型。

- 数组中的每一个元素都按照顺序依次存储，这个顺序号既代表着元素的存储位置，也是数组元素的唯一标识。我们把这个标识称之为 **数组下标** 。

  注意，数组下标从 `0` 开始。

- 填充数组中的每一个元素的过程称为 **数组初始化**。也就是说 **数组初始化** 就是为数组中的每一个元素赋值。

- 可以**更新**或**修改**数组元素的值，但**不能删除**数组元素。如果要删除功能，你可以将它的值赋值为 0 或其它表示删除的值。

## 声明语法

### 简单的数组

1.最基本的语法，指定每一个元素的初始值。

```rust
let arr:[i32;4] = [10,20,30,40];
```

2.省略数组类型的语法。

> 不推荐这种，没有显式声明类型（类型会根据第一个值的默认类型确定），一旦超过类型长度或有多个类型会在编译期报错

```rust
let arr = [10,20,30,40];
```

3.指定默认初始值的语法，这种语法有时候称为 **默认值初始化**。

例如下面的代码为每一个元素指定初始值为 `-1`

```rust
let arr:[i32;4] = [-1;4];
```

### 多维数组

数组的元素仍然可以是数组，这样就构成了多维数组

#### 二维数组

```rust
let a_105 = [[1,2],[3,4]];
let a_106:[[i32;2];2] = [[1,2],[3,4]];
```

#### 三维数组

```rust
let mut a_107 : [[[i32;2];2];2] = [[[0x55AA;2];2];2];
println!("{:?}",a_107);
```

运行结果

```shell
[[[21930, 21930], [21930, 21930]], [[21930, 21930], [21930, 21930]]]
```

### 可变数组

使用 `let` 声明的变量，默认是只读的，数组也不例外。也就是说，默认情况下，数组是不可变的。

数组的不可变，表现为两种形式：变量不可重新赋值为其它数组、数组的元素不可以修改。

如果要让数组的元素可以修改，就需要添加 `mut` 关键字。

```rust
fn main(){
   let mut arr:[i32;4] = [10,20,30,40];
   arr[1] = 0;
   println!("{:?}",arr);
}
```

### 数组声明和常量

数组 ( array ) 的长度必须在编译时就是固定的已知的。

声明数组时长度必须指定为**整数字面量**或者**整数常量**。

如果数组长度是一个变量，则会报编译错误。

```rust
fn main() {
   let N: usize = 20;
   let arr = [0; N]; //错误: non-constant used with constant
   print!("{}",arr[10])
}
```

编译上面的 Rust 代码报错

```shell
error[E0435]: attempt to use a non-constant value in a constant
 --> main.rs:3:18
  |
3 |    let arr = [0; N]; //错误: non-constant used with constant
  |                  ^ non-constant value

error: aborting due to previous error

For more information about this error, try `rustc --explain E0435`.
```

注意，虽然 N 默认是只读的，但它仍然是一个变量，只不过是一个只读变量而已，只读变量不是常量。因为变量的值是在运行时确定的，而常量的值是在编译器确定的。

变量不可用做数组的长度。

如果我们将 `let` 关键字修改为 `const` 关键字，编译就能通过了。

```rust
fn main() {
   const N: usize = 20; 
   // 固定大小
   let arr = [0; N];

   print!("{}",arr[10])
}
```

> `usize` 是一个指针所占用的大小。它的实际大小取决于你编译程序的 cpu 体系结构。

## 数组长度 len()

Rust 为数组提供了 `len()` 方法用于返回数组的长度。

`len()` 方法的返回值是一个整型。

```Rust
fn main() {
   let arr:[i32;4] = [-1;4];
   println!("array size is :{}",arr.len());
}
```

## 访问数组元素

### 访问元素

数组是可以在栈上分配的已知固定大小的单个内存块。可以使用索引访问数组的元素

```rust
fn main() {
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
}
```

多维数组

```rust
let mut a_105 = [[1,2],[3,4]];
a_105[0]= [5,5];
a_105[1][1] = 1i32;
println!("{:?}",a_105);

let mut a_106 : [[[i32;2];2];2] = [[[0x55AA;2];2];2];
a_106[0][0][0]= 0x10;
println!("{:?}",a_106);
```

### 下标越界

```rust
use std::io;

fn main() {
    let a = [1, 2, 3, 4, 5];

    println!("Please enter an array index.");

    let mut index = String::new();

    io::stdin()
        .read_line(&mut index)
        .expect("Failed to read line");

    let index: usize = index
        .trim()
        .parse()
        .expect("Index entered was not a number");

    let element = a[index];

    println!(
        "The value of the element at index {} is: {}",
        index, element
    );
}
```

如果使用 `cargo run` 来运行此代码并输入 0、1、2、3 或 4，则程序将打印数组对应索引的值。如果输入的是超出数组末尾的数字，例如 10，则会看到类似以下的输出：

```shell
thread 'main' panicked at 'index out of bounds: the len is 5 but the index is 10', src/main.rs:19:19
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

该程序在索引操作中使用无效值时导致**运行时**（*runtime*）错误。程序退出并显示错误消息，未执行后面的 `println!` 语句。

当你尝试使用索引访问元素时，Rust 将检查你指定的索引是否小于数组长度。如果索引大于或等于数组长度，Rust 会出现 `panic`。这种检查必须在运行时进行，尤其是在这种情况下，因为编译器可能无法知道用户之后运行代码时将输入什么值。

>`panic!()` 会导致程序立即退出，并在退出时向它的调用者反馈退出原因。

## 切片（slice）

slice（中文有“切片”之意） 类型和数组类似，但 slice 类型的大小在编译期间是不确定的。

相反， slice 是一个双字对象（two-word object），第一个字是一个指向数据的指针，第二个字是切片的 长度。

slice 可以用来借用数组的一部分。

> slice 的类型标记为 `&[T]`。

一个 **切片（slice）** 就是指向一段内存的指针。

因此切片可用于访问内存块中连续区间内的数据。

一般情况下，能够在内存中连续区间存储数据的数据结构有: 数组 array、向量 vector、字符串 string。

也就是说，**切片** 可以和数组、向量、字符串一起使用，它使用 数字索引 （类似于数组的下标索引）来访问它所指向的数据。

例如，**切片** 可以和字符串一起使用，切片 可以指向字符串中连续的一部分。这种 字符串切片 实际上是指向字符串的指针。因为是指向字符串的连续区间，所以我们要指定字符串的开始和结束位置。

访问切片内容的时候，下标索引是从 0 开始的。

### 定义切片

```rust
let sliced_value = &data_structure[start_index..end_index]
```

定义切片时的几个注意事项：

1. `[start_index..end_index] `是一个左闭又开区间 `[start_index,end_index)`
2. 要注意区间的语法两个点 `..` 。
3. start_index 的最小值为 `0`，这是因为数组、向量、字符串它们的下标访问方式也是从 0 开始的。
4. end_index 的最大值是数组、向量、字符串的长度。
5. end_index 所表示的索引的字符并不包含在切片里面。

### 数组切片实例

```rust
fn main() {
    let arr = [1, 3, 5, 7, 9];
    let part = &arr[0..3];
    for i in part.iter() {
        println!("{}", i);
    }
}
```

> 如果切片从`0`开始，则可以省略前面的`start_index`;例如: `let part = &arr[..3];`
>
> 如果切片一直取到最后，则可以省略`end_index`例如:`let part = &arr[3..];`
>
> 如果想取全部，则都可以省略，例如:`let part = &arr[..];`

## 循环遍历数组

### for in 循环遍历数组

因为数组的长度在编译时就时已知的，因此我们可以使用 `for ... in` 语句来遍历数组。

```rust
fn main(){
   let arr:[i32;4] = [10,20,30,40];
   println!("array is {:?}",arr);
   println!("array size is :{}",arr.len());

   for index in 0..4 {
      println!("index is: {} & value is : {}",index,arr[index]);
   }
}
```

我们也可以使用变量来接收数组长度，从而决定循环范围

```rust
fn main() {
    let arr:[i32;4] = [10,20,30,40];
    println!("array is {:?}",arr);
    println!("array size is :{}",arr.len());

    let len = arr.len();//--接收数组长度
 
    for index in 0..len {//--循环范围
       println!("index is: {} & value is : {}",index,arr[index]);
    }
}
```

### 迭代数组 iter()

我们可以使用 `iter()` 函数为数组生成一个迭代器。

然后就可以使用 `for in` 语法来迭代数组。

```rust
fn main(){
    let arr:[i32;4] = [10,20,30,40];
    println!("array is {:?}",arr);
    println!("array size is :{}",arr.len());

    for val in arr.iter(){
        println!("value is :{}",val);
    }
}
```

## 数组作为函数参数

数组可以作为函数的参数。而传递方式有 **传值传递** 和 **引用传递** 两种方式。

**传值传递** 就是传递数组的一个副本给函数做参数，函数对副本的任何修改都不会影响到原来的数组。

**引用传递** 就是传递数组在内存上的位置给函数做参数，因此函数对数组的任何修改都会影响到原来的数组。

### 传值传递

```rust
fn main() {
   let arr = [10,20,30];
   update(arr);

   println!("Inside main {:?}",arr);
}
fn update(mut arr:[i32;3]){
   for i in 0..3 {
      arr[i] = 0;
   }
   println!("Inside update {:?}",arr);
}
```

编译运行以上 Rust 代码，输出结果如下

```shell
Inside update [0, 0, 0]
Inside main [10, 20, 30]
```

### 引用传递

```rust
fn main() {
   let mut arr = [10,20,30];
   update(&mut arr);
   println!("Inside main {:?}",arr);
}
fn update(arr:&mut [i32;3]){
   for i in 0..3 {
      arr[i] = 0;
   }
   println!("Inside update {:?}",arr);
}
```

编译运行以上 Rust 代码，输出结果如下

```shell
Inside update [0, 0, 0]
Inside main [0, 0, 0]
```

