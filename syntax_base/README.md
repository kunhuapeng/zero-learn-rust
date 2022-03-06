# Rust语法基础

> 基本类型： bool，char, i8,...,usize,i64, f32,f64, 数组[T:N], [T]切片, str字符串切片&str,  (t,u)序列，fn(i32)->i32函数

> 变量声明和不可变性： let xx=xx, let mut xx=xx;

> 函数: fn xx(x:i32,y:u32){}

> 闭包: |x,y|{ x*y}

> 字符串 &str, string类型 &创建指向任何类型的指针

> 条件if else if

> match表达式 match xx{ a=> xx, b=>xx, other=>xxx;}

> 循环loop{}

> 自定义数据类型 struct, struct color(u8,u8,u8)

> 枚举 enum{x,x1,x3}

> 类型上的函数和方法 impl
>
> 1. 关联方法
> 2. 实例方法 self，&self，&mut self

> impl块和枚举
>
> module，import和use语句
>
> 1. 每个rust都有一个root模块 main.rs或者lib。rs
> 2. 模块可以在其他模块内部声明，以文件或目录组织
> 3. 模块中块默认私有，pub暴露

> 集合
>
> 1. 数组 [T:N]
> 2. 元组 xx:(u8,&str)

> 列表 vec![]

> 键值对: std::collections::HashMap

> 切片：&[T]

> 迭代器 iter