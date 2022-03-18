# 打印圣诞颂歌 的歌词

## 概述

打印圣诞颂歌 “The Twelve Days of Christmas” 的歌词，利用歌曲中的重复部分（编写循环）。

## 分析

> 每行歌词的第一句都是： `On the *first* day of Christmas, my true love sent to me:`，只有日期不一样

> 每行歌词的第二句都是当天和前几天的礼物，依次累加，只有些许差异。

## 代码

```rust
fn main() {
    let days = ["first", "second", "third", "fourth", "fifth", "sixth", "seventh", "eighth", "ninth", "tenth", "eleventh", "twelfth"];
    let gifts = [
        "And a partridge in a pear tree.",
        "Two turtle doves, ",
        "Three French hens,",
        "Four calling birds,",
        "Five golden rings;",
        "Six geese a-laying,",
        "Seven swans a-swimming,",
        "Eight maids a-milking,",
        "Nine ladies dancing,",
        "Ten lords a-leaping,",
        "Eleven pipers piping,",
        "Twelve drummers drumming,",
    ];

    for (i, day) in days.iter().enumerate() {
        let mut the_gifts = String::new();
        if i == 0 {
            the_gifts.push_str("A partridge in a pear tree.");
        } else {
            for n in (0..=i).rev() {
                the_gifts.push_str(gifts[n]);
            }
        }
        println!("On the {} day of Christmas, my true love sent to me: {}", day, the_gifts);
    }
}
```

