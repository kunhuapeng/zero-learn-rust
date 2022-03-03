# 运算符

## 运算符

**运算符** 用于对数据执行一些操作。

被 **运算符** 执行操作的数据我们称之为 **操作数**。

## 一元运算符

一元操作符是专门对一个 Rust 元素进行操作的运算符，主要包括以下几个：

> `-` ：取负，专门用于数值类型。实现了 `std::ops::Neg`。

> `*` ：解引用。实现了 `std::ops::Deref` 或 `std::ops::DerefMut`。

> `!` ：取反。例如 `!false` 相当于 `true`。
> 有意思的是，如果这个操作符对数字类型使用，会将其每一位都置反！也就是说，对一个 `1u8` 进行 ! 操作，将得到一个 `254u8`。`实现了 std::ops::Not`。

> `&` 和 `&mut` ：租借，borrow。向一个 owner 租借其使用权，分别租借一个只读使用权和读写使用权。

## 二元运算符

### 算术运算符

算术运算符就是我们日常所使用的 **加减乘除求余** 五则运算。

我们假设 a = 10 且 b = 5。

| 名称 | 运算符 | 范例             |
| ---- | ------ | ---------------- |
| 加   | +      | a+b 的结果为 15  |
| 减   | -      | a-b 的结果为 5   |
| 乘   | *      | a*b 的结果为 50  |
| 除   | /      | a / b 的结果为 2 |
| 求余 | %      | a % b 的结果为 0 |

> Rust 语言不支持自增自减运算符 `++` 和 `--`。

> 操作符两边的操作数**类型**必须**相同**。
>
> Rust **除法**运算，如果操作数为**整型**，且不能整除，则结果自动**向下取整**。

例如：

```rust
fn main() {
    print!("{}", 19 / 10);//--结果为：1
}
```

**范例：**

```rust
fn main() {
   let num1 = 10 ;
   let num2 = 2;
   let mut res:i32;

   res = num1 + num2;
   println!("Sum: {} ",res);

   res = num1 - num2;
   println!("Difference: {} ",res) ;

   res = num1*num2 ;
   println!("Product: {} ",res) ;

   res = num1/num2 ;
   println!("Quotient: {} ",res);

   res = num1%num2 ;
   println!("Remainder: {} ",res);
}
```

运行结果：

```shell
Sum: 12
Difference: 8
Product: 20
Quotient: 5
Remainder: 0
```

### 关系运算符

>关系运算符测试或定义两个实体之间的关系类型。
>
>关系运算符用于比较两个或多个值之间的关系，是大于，是等于还是小于。
>
>关系运算符的返回结果为 **布尔类型**。

我们假设 A = 10 且 B = 20。

| 名称     | 运算符 | 说明                                                     | 范例                |
| -------- | ------ | -------------------------------------------------------- | ------------------- |
| 大于     | >      | 如果左操作数大于右操作数则返回 true 否则返回 false       | (A > B) 返回 false  |
| 小于     | <      | 如果左操作数小于于右操作数则返回 true 否则返回 false     | (A < B) 返回 true   |
| 大于等于 | >=     | 如果左操作数大于或等于右操作数则返回 true 否则返回 false | (A >= B) 返回 false |
| 小于等于 | <=     | 如果左操作数小于或等于右操作数则返回 true 否则返回 false | (A <= B) 返回 true  |
| 等于     | ==     | 如果左操作数等于右操作数则返回 true 否则返回 false       | (A == B) 返回 true  |
| 不等于   | !=     | 如果左操作数不等于右操作数则返回 true 否则返回 false     | (A != B) 返回 false |

**范例：**

```rust
fn main() {
   let A:i32 = 10;
   let B:i32 = 20;

   println!("Value of A:{} ",A);
   println!("Value of B : {} ",B);

   let mut res = A>B ;
   println!("A greater than B: {} ",res);

   res = A<B ;
   println!("A lesser than B: {} ",res) ;

   res = A>=B ;
   println!("A greater than or equal to B: {} ",res);

   res = A<=B;
   println!("A lesser than or equal to B: {}",res) ;

   res = A==B ;
   println!("A is equal to B: {}",res) ;

   res = A!=B ;
   println!("A is not equal to B: {} ",res);
}
```

运行结果：

```shell
Value of A:10
Value of B : 20
A greater than B: false
A lesser than B: true
A greater than or equal to B: false
A lesser than or equal to B: true
A is equal to B: false
A is not equal to B: true
```

### 逻辑运算符

>逻辑运算符用于组合两个或多个条件。
>
>逻辑运算符的返回结果也是布尔类型。

我们假设 A = 10 且 B = 20。

| 名称   | 运算符 | 说明                                                     | 范例                                |
| ------ | ------ | -------------------------------------------------------- | ----------------------------------- |
| 逻辑与 | &&     | 两边的条件表达式都为真则返回 true 否则返回 false         | (A > 10 && B > 10) 的结果为 `false` |
| 逻辑或 | \|\|   | 两边的条件表达式只要有一个为真则返回 true 否则返回 false | (A > 10 \|\| B >10) 的结果为 `true` |
| 逻辑非 | !      | 如果表达式为真则返回 false 否则返回 true                 | !(A >10) 的结果为 `true`            |

**范例：**

```rust
fn main() {
   let a = 20;
   let b = 30;

   if (a > 10) && (b > 10) {
      println!("true");
   }
   let c = 0;
   let d = 30;

   if (c>10) || (d>10){
      println!("true");
   }
   let is_elder = false;

   if !is_elder {
      println!("Not Elder");
   }
}
```

运行结果

```shell
true
true
Not Elder
```

### 位运算符

假设变量 A = 2 且变量 B = 3。

A 的二进制格式为

0 0 0 0 0 0 1 0

B 的二进制位格式为

0 0 0 0 0 0 1 1

| 名字 | 运算符 | 说明                                           | 范例               |
| ---- | ------ | ---------------------------------------------- | ------------------ |
| 位与 | &      | 相同位都是 1 则返回 1 否则返回 0               | (A & B) 结果为 `2` |
| 位或 | \|     | 相同位只要有一个是 1 则返回 1 否则返回 0       | (A \| B) 结果为 3  |
| 异或 | ^      | 相同位不相同则返回 1 否则返回 0                | (A ^ B) 结果为 1   |
| 位非 | !      | 把位中的 1 换成 0 ， 0 换成 1                  | (!B) 结果 -4       |
| 左移 | <<     | 操作数中的所有位向左移动指定位数，右边的位补 0 | (A << 1) 结果为 4  |
| 右移 | >>     | 操作数中的所有位向右移动指定位数，左边的位补 0 | (A >> 1) 结果为 1  |

**范例：**

```rust
   let a:i32 = 2;     // 二进制表示为 0 0 0 0 0 0 1 0
   let b:i32 = 3;     // 二进制表示为 0 0 0 0 0 0 1 1

   let mut result:i32;

   result = a & b;
   println!("(a & b) => {} ",result);

   result = a | b;
   println!("(a | b) => {} ",result) ;

   result = a ^ b;
   println!("(a ^ b) => {} ",result);

   result = !b;
   println!("(!b) => {} ",result);

   result = a << b;
   println!("(a << b) => {}",result);

   result = a >> b;
   println!("(a >> b) => {}",result);
}
```

运行结果

```shell
(a & b) => 2
(a | b) => 3
(a ^ b) => 1
(!b) => -4
(a << b) => 16
(a >> b) => 0
```

