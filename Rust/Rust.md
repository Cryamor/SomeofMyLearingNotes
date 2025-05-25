# Rust

编译型语言，`rustc`命令编译`.rs`程序

Cargo 命令：

```
cargo new <project-name>
cargo build
cargo run
```

## 基本语法

### 输出

`print!()` `println()`

```rust
println!("a is {0}, a again is {0}", a); 
```

转义`{{` `}}` 其他与C相同

### 变量

Rust是强类型语言，但具有自动判断变量类型的能力
Rust是静态类型语言，在变量声明时可以显式指定类型
基本类型: `i32` `u32` `f64` `bool` `char`

```rust
let a = 1;       // 不可变变量
let mut b = 1;   // 可变变量
const c = 1;     // 常量
let x: i32 = 2;
let y: f64 = 3.14;
let is_true: bool = true;
let letter: char = 'A';
```

不可变变量与常量的区别是可以用`let`重新绑定

### 函数

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

如果没有返回值，类型默认为 `()`（空元组）。

### 控制流

```rust
// if-else
let a = 0;
if a < 1 {
    println!("0");
} else {
    println!("1");
}

// while
let mut b = 3;
while b != 0 {
    println!("{}", b);
    b -= 1;
}

// for
for c in 1..4 {
    println!("{}", c);
}

// loop 无限循环
loop {
    b += 1;
    if b == 10 {
        break;
    }
}
```

## 所有权

规则：

1. Rust 中的每个值都有一个变量，称为其所有者。
2. 一次只能有一个所有者。
3. 当所有者不在程序运行范围时，该值将被删除。

## String str

在 Rust 中有两种常用的字符串类型：str 和 String。str 是 Rust 核心语言类型字符串切片（String Slice），常常以引用的形式出现（&str）。

凡是用双引号包括的字符串常量整体的类型性质都是 **&str**：

```rust
let s = "hello";  // &str

let s1 = String::from("hello");
let s2 = &s1[..];
```

String 类型是 Rust 标准公共库提供的一种数据类型，它的功能更完善——它支持字符串的追加、清空等实用的操作。String 和 str 除了同样拥有一个字符开始位置属性和一个字符串长度属性以外还有一个容量（capacity）属性。

String 和 str 都支持切片，切片的结果是 &str 类型的数据。

let slice = &s[0..3];

有一个快速的办法可以将 String 转换成 &str：
