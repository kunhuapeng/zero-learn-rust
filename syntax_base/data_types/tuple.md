# 元组（tuple）

## 元组

元组（Tuple）是一种组合类型，使用小括号来表示，其中每个值的类型可以不相同。

> **元组有着固定的长度**。而且一旦定义，就不能再增长或缩小。
>
> **元组的下标从 `0` 开始**。

## 定义语法

```rust
let tuple_name:(data_type1,data_type2,data_type3) = (value1,value2,value3);
```

也可以忽略数据类型

```rust
let tuple_name = (value1,value2,value3);
```

> 元组的定义就是使用一对小括号 `()` 把所有元素放在一起，元素之间使用逗号 `,` 分隔。

> 定义元组数据类型的时候也是一样的。

> 如果显式指定了元组的数据类型，那么数据类型的个数必须和元组的个数相同，否则会报错。

### 元组嵌套

元组的定义是可以嵌套的

```rust
let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);
println!("tuple of tuples: {:?}", tuple_of_tuples);
```

### 一个元素的元组

当元组只有一个元素时，元素后面必须跟一个逗号,用来区分与普通数据的差别；

```rust
fn main() {
    let a = (2,);
    println!("{:?},{:?}", a, a.0);//--运行结果： (2,)   ,   2
}
```

如果不写`,`的话，会被认为是普通数据，而不是元组，运行的时候会出现警告。

例如：

```rust
fn main() {
    let a = ("只有一个元素");
    println!("{:?}", a);
}
```

运行时出现警告：

```shell
warning: unnecessary parentheses around assigned value
  --> src\main.rs:14:13
   |
14 |     let a = ("只有一个元素");
   |             ^              ^
   |
   = note: `#[warn(unused_parens)]` on by default
help: remove these parentheses
   |
14 -     let a = ("只有一个元素");
14 +     let a = "只有一个元素";
   |

warning: `my_test` (bin "my_test") generated 1 warning
    Finished dev [unoptimized + debuginfo] target(s) in 1.92s
     Running `target\debug\my_test.exe`
"只有一个元素"
```

### 单元值

没有任何值的元组 `()` 是一种特殊的类型，只有一个值，也写成 `()` 。
该类型被称为 **单元类型**（*unit type*），而该值被称为 **单元值**（*unit value*）。
如果表达式不返回任何其他值，则会隐式返回单元值。

## 访问元组元素

### 1.索引数字

可以使用 `元组名.索引数字` 来访问元组中相应索引位置的元素。索引从 `0` 开始。

```rust
fn main() {
   let tuple:(i32,f64,u8) = (-325,4.9,22);
   println!("integer is :{:?}",tuple.0);
   println!("float is :{:?}",tuple.1);
   println!("unsigned integer is :{:?}",tuple.2);
}
```

### 2.元组解构赋值 ( destructing ) =

**解构赋值 ( destructing )** 就是把 **元组 ( tuple )** 中的每一个元素按照顺序一个一个赋值给变量。

**语法格式**

```rust
let (age,is_male,cgpa) = (30,true,7.9);
```

**范例**

```rust
let tup = (2.0, 3.4, 1.1);
let (x, y, z) = tup;
println!(" x : {},y : {},z : {}", x,y,z);

//或直接这样声明
let (x, y, z) = (1,2,3);
println!(" x : {},y : {},z : {}", x,y,z);
//特殊类型需要特殊
let (x, mut y, z) = (2.2,2,[1,3]);
y+=1;
println!(" x : {},y : {},z : {}", x,y,z[0]);
```

### 修改元组元素

元组元素的类型和值是可变的；

> 想要修改元组的元素，元组声明时必须用`mut`标明是可变的。
>
> 元组的元素的**值可以修改**，元素的**类型不可修改**，只能强制指定一次。
>
> 未强制指定类型的元素，会根据**类型推断**标明类型，也可以在重新赋值的时候，强制标明类型。
>
> 赋值时强制指定的类型，不能与元素原来的值冲突。例如，原来元素值为“1”，那么，哪怕原来未指定类型，也不可以被赋值为-1.

#### 修改元素的值：

```rust
fn main() {
    let mut tuple = (1, 2, 3, 4);
    tuple.0 = -1;
    println!("{:?}", tuple);//(-1, 2, 3, 4)
    tuple.0 = 100;
    println!("{:?}", tuple);//(100, 2, 3, 4)
}
```

#### 指定元素的类型：

>只有元组声明时未指定类型，该元素的类型才可以被重新指定。
>
>强指定的类型不可以与原类型冲突。
>
>只能强制指定一次。

```rust
let mut tuple_d: (u16,bool,f64) = (255,true,3.14);
let  num_u16_1:u16 = 65535;
tuple_d.0 = num_u16_1;//error  mismatched types  类型不可变
println!("tuple_d is {:?}", tuple_d);

let mut tuple_e = (255,true,3.14);//如果只有这一行，默认类型i32,bool,f64；但是后面有元素之改变，因此0的类型推断为u16
tuple_e.0 = num_u16_1;// 0 的类型推断改变了：  u16,bool,f64
println!("tuple_e is {:?}", tuple_e);
let  num_u32_1:u32 = 4294967295;//类型已经确认，不可再次改变类型
tuple_e.0 = num_u32_1;//error  mismatched types
```

## 元组使用

### 作为函数的参数、返回值

元组也可以作为函数的参数。

```rust
fn function_name(tuple_name:(type1,type2,type3)) -> (type4, type5) {}
```

**范例**

```rust
fn main(){
   let b:(i32,bool,f64) = (110,true,10.9);
   print(b);
}

// 使用元组作为参数
fn print(x:(i32,bool,f64)){
   println!("Inside print method");
   println!("{:?}",x);
}
```

### 打印元组

```rust
// 元组嵌套
let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);
// 元组打印
println!("tuple of tuples: {:?}", tuple_of_tuples);
```

### 元组遍历

### 元组使用范例

```rust
// 元组可以充当函数的参数和返回值
fn reverse(pair: (i32, bool)) -> (bool, i32) {
    // 可以使用 `let` 来绑定元组的各个变量
    let (integer, boolean) = pair;//--解构

    (boolean, integer)
}

#[derive(Debug)]
struct Matrix(f32, f32, f32, f32);//--元组结构体

fn main() {
    // 包含各种不同类型的元组
    let long_tuple = (1u8, 2u16, 3u32, 4u64,
                      -1i8, -2i16, -3i32, -4i64,
                      0.1f32, 0.2f64,
                      'a', true);

    // 通过元组的索引来访问具体的值
    println!("long tuple first value: {}", long_tuple.0);
    println!("long tuple second value: {}", long_tuple.1);

    // 元组也可以充当元组的元素（嵌套）
    let tuple_of_tuples = ((1u8, 2u16, 2u32), (4u64, -1i8), -2i16);

    // 元组可以打印
    println!("tuple of tuples: {:?}", tuple_of_tuples);

    let pair = (1, true);
    println!("pair is {:?}", pair);

    println!("the reversed pair is {:?}", reverse(pair));

    // 创建单元素元组需要一个额外的逗号，这是为了和括号包含的普通数据作区分。
    println!("one element tuple: {:?}", (5u32,));
    println!("just an integer: {:?}", (5u32));

    // 解构元组，将值赋给创建的绑定变量
    let tuple = (1, "hello", 4.5, true);

    let (a, b, c, d) = tuple;
    println!("{:?}, {:?}, {:?}, {:?}", a, b, c, d);

    let matrix = Matrix(1.1, 1.2, 2.1, 2.2);
    println!("{:?}", matrix)

}
```

