# while循环

## 概述

条件表达式为真时，执行语句体。当你不确定应该循环多少次时可选择 while。

- while 循环是条件循环
- 可使用 break 提前结束循环
- 可使用 continue 跳过本次循环

**示例**

```rust
fn main() {
    let mut i = 3;
    while i != 0 {
        println!("输出: {}", number);
        i -= 1;
    }
}
```

当条件为真，执行循环。当条件不再为真，调用 `break` 停止循环。这个循环类型可以通过组合 `loop`、`if`、`else` 和 `break` 来实现。

这种结构消除了很多使用 `loop`、`if`、`else` 和 `break` 时所必须的嵌套，这样更加清晰。当条件为真就执行，否则退出循环。