# 斐波那契数列

## 概述

斐波那契数列（Fibonacci sequence），又称黄金分割数列，因数学家莱昂纳多·斐波那契（Leonardo Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……

在数学上，斐波那契数列以如下被以递推的方法定义：*F*(0)=0，*F*(1)=1, *F*(n)=*F*(n - 1)+*F*(n - 2)（*n* ≥ 2，*n* ∈ N*）

## 要求

使用rust来生成一个 n 阶斐波那契数列。

## 分析

> 数列计算规律 *F*(0)=0，*F*(1)=1, *F*(n)=*F*(n - 1)+*F*(n - 2)（*n* ≥ 2，*n* ∈ N*）

因此，我们在计算第`n`阶的时候，需要知道第`n-1`和第`n-2`阶的值，因此有两种解决方案

1. 用动态数组来存储第1~n阶的值，最终输出该数组。
2. 使用递归来计算第n阶的值，循环存入数组，最后输出。

显然，如果要输出整个数列的话，方法1更快捷

## 解答

### 创建项目

```shell
cargo new fibonacci
```

### 使用动态数组

**代码**

```rust
fn main() {
    let n = 10;
    let s = fibonacci(n);
    println!("{}阶斐波那契数列：{:?}", n, s);
}

fn fibonacci(n: usize) -> Vec<u32> {
    let mut result = Vec::new();

    for i in 0..n {
        if i == 0 || i == 1 {
            result.push(1);
        } else {
            let x = result[i - 2];
            let y = result[i - 1];
            let n = x + y;
            result.push(n);
        }
    };
    
    result
}
```

### 使用递归

**代码**

```rust
fn main() {
    let n = 10;
    let mut result = Vec::new();

    for i in 0..n {
        result.push(fibonacci(i));
    }

    println!("{}阶斐波那契数列：{:?}", n, result);
}


fn fibonacci(n: usize) -> u32 {
    if n == 0 || n == 1 {
        return 1;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);//--递归
    }
}
```

### 使用结构体

**代码**

```rust
#[derive(Debug)]
struct Fibonacci {
    series: Vec<u32>,
}

impl Fibonacci {
    fn new(len: usize) -> Fibonacci {
        let mut fbncc = Fibonacci{
            series: Vec::new()
        };
        for i in 0..len {
            fbncc.series.push(if i == 0 || i == 1 {
                1
            } else {
                fbncc.series[i - 1] + fbncc.series[i - 2]
            });
        }
        return fbncc;
    }
}

fn main() {
    let n = 10;
    let s = Fibonacci::new(10);
    println!("{}阶斐波那契数列：{:?}", n, s.series);
}
```

