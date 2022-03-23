# `struct`结构体

## 概述

- 结构体(*`struct`*) 是一个自定义数据类型
- 允许你命名和包装多个相关的值, 形成一个有意义的组合

## 结构体定义

- 使用`struct`关键字定义一个结构体，并为整个结构体提供一个名字
- 结构体的成员叫做**字段**(*`field`*)

```rust
struct User {
    username: String,
    password: String,
    email: String,
    age: u32,
}
```

## 实例化结构体

- 必须为每个字段指定具体的值
- 无需按照字段声明的顺序进行指定

```rust
let user = User {
    username: String::from("admin"),
    password: String::from("Admin&rust"),
    email: String::from("admin@qq.com"),
    age: 18,
};
```

### 结构体数据的所有权

结构体必须掌握字段值所有权，因为结构体失效的时候会释放所有字段。

这就是为什么本章的案例中使用了 String 类型而不使用 &str 的原因。

但这不意味着结构体中不定义引用型字段，这需要通过"生命周期"机制来实现。

```rust
struct User {
    active: bool,
    username: &str,
    email: &str,
    sign_in_count: u64,
}

fn main() {
    let user1 = User {
        email: "someone@example.com",
        username: "someusername123",
        active: true,
        sign_in_count: 1,
    };
}
```

运行结果：编译器会抱怨它需要生命周期标识符

```shell
$ cargo run
   Compiling structs v0.1.0 (file:///projects/structs)
error[E0106]: missing lifetime specifier
 --> src/main.rs:3:15
  |
3 |     username: &str,
  |               ^ expected named lifetime parameter
  |
help: consider introducing a named lifetime parameter
  |
1 ~ struct User<'a> {
2 |     active: bool,
3 ~     username: &'a str,
  |

error[E0106]: missing lifetime specifier
 --> src/main.rs:4:12
  |
4 |     email: &str,
  |            ^ expected named lifetime parameter
  |
help: consider introducing a named lifetime parameter
  |
1 ~ struct User<'a> {
2 |     active: bool,
3 |     username: &str,
4 ~     email: &'a str,
  |

For more information about this error, try `rustc --explain E0106`.
error: could not compile `structs` due to 2 previous errors

```

## 访问结构体字段

- 使用点标记法获取字段
- 如果需要改变结构体内的值, 我们需要将结构体声明为可变的

```rust
let mut user = User {
    username: String::from("admin"),
    password: String::from("Admin&rust"),
    email: String::from("admin@qq.com"),
    age: 18,
};
user.email = String::from("admin@139.com");
```

> 注意整个实例必须是可变的；Rust 并不允许只将某个字段标记为可变。

## 作为函数返回值

可以在函数体的最后一个表达式中构造一个结构体的新实例，来隐式地返回这个实例。

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn build_user(email: String, username: String) -> User {
    User {
        email: email,
        username: username,
        active: true,
        sign_in_count: 1,
    }
}

fn main() {
    let user1 = build_user(
        String::from("someone@example.com"),
        String::from("someusername123"),
    );
}
```

## 字段同名简化写法

变量与字段同名时: 可以省略字段名

```rust
fn build_user(username: &str, password: &str, email: &str, age: u32) -> User {
    User {
        username: String::from(username),
        password: String::from(password),
        email: String::from(email),
        age,
    }
}
```

## 从其他实例更新字段

使用旧实例的大部分值但改变其部分值来创建一个新的结构体实例通常很有用。这可以通过**结构体更新语法**（*struct update syntax*）实现。

使用结构体更新语法，我们可以通过更少的代码来达到相同的效果，

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}

fn main() {

    let user1 = User {
        email: String::from("someone@example.com"),
        username: String::from("someusername123"),
        active: true,
        sign_in_count: 1,
    };

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}

```

> `..` 语法指定了剩余未显式设置值的字段应有与给定实例对应字段相同的值。

> 请注意，结构更新语法就像带有 `=` 的赋值，它移动了数据。

*在这个例子中，我们在创建 `user2` 后不能再使用 `user1`，因为 `user1` 的 `username` 字段中的 `String` 被移到 `user2` 中。如果我们给 `user2` 的 `email` 和 `username` 都赋予新的 `String` 值，从而只使用 `user1` 的 `active` 和 `sign_in_count` 值，那么 `user1` 在创建 `user2` 后仍然有效。因为`active` 和 `sign_in_count` 的类型是实现 `Copy` trait 的类型。*

## 元组结构体

元组结构体有着结构体名称提供的含义，但没有具体的字段名，只有字段的类型。当你想给整个元组取一个名字，并使元组成为与其他元组不同的类型时，元组结构体是很有用的，这时像常规结构体那样为每个字段命名就显得多余和形式化了。

- 类似于元组的结构体就叫做元组结构体(*tuple struct*)
- 元组结构体拥有名字, 字段(元素)没有名字

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

> 要定义元组结构体，以 `struct` 关键字和结构体名开头并后跟元组中的类型。

> 你定义的每一个结构体有其自己的类型，即使结构体中的字段有着相同的类型。

## 类单元结构体

- 没有任何字段的控结构体叫做**类单元结构体**(*unit-like structs*)
- 类单元结构体适用于在某个类型上实现某个**trait** 但是没有想要储存的数据

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

> 要定义 `AlwaysEqual`，我们使用 `struct` 关键字，我们想要的名称，然后是一个分号。不需要花括号或圆括号！
>
> 然后，我们可以以类似的方式在 `subject` 变量中获得 `AlwaysEqual` 的实例：使用我们定义的名称，不需要任何花括号或圆括号。