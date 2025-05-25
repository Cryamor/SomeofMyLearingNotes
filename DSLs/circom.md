# Circom 

https://docs.circom.io/

## Circom 流程

``` 

pragma circom 2.0.0;

template Multiplier2() {
   signal input a;
   signal input b;
   signal output c;
   c <== a*b;
}

component main = Multiplier2();

```

- `pragma`指定编译器版本
- `template`定义电路，需要定义输入和输出信号。先声明信号，再声明约束
- `c <== a * b`等效于`a * b ==> c`
- 有效的 R1CS 必须每个约束（在 R1CS 中的一行，在 Circom 中 `<==`）只有一个乘法。如`c <= a * b * d`会报错


保存在`multiplier2.circom`文件中，编译：
```
circom multiplier2.circom --r1cs --wasm --sym --c
```
- `--r1cs`：生成文件`multiplier2.r1cs`，其中包含二进制格式的电路的R1CS。
- `--wasm`：生成包含Wasm代码（`multiplier2.wasm`）和生成见证所需的其他文件的目录（`multiplier2_js`）。
- `--sym` ：生成文件`multiplier2.sym`，是调试或在注释模式下打印约束系统所需的符号文件
- `--c` ：生成目录`multiplier2_cpp`，其中包含编译 C 代码以生成见证所需的多个文件（`multiplier2.cpp`、`multiplier2.dat` 和每个编译程序的其他通用文件，如 `main.cpp`、`MakeFile` 等）
- `-o`制定这些文件的创建目录

查看`.r1cs`中的二进制数据：
```
snarkjs r1cs print multiplier2.r1cs
```
输出：
```
[INFO]  snarkJS: [ 21888242871839275222246405745257275088548364400416034343698204186575808495616main.a ] * [ main.b ] - [ 21888242871839275222246405745257275088548364400416034343698204186575808495616main.c ] = 0
```
这个巨大的数字实际是-1，R1CS方程等价于：
```
-1 * a * b - (-1) * c = 0
```

在`_js`文件夹下创建一个`input.json`文件：
``` json
{"a": "2","b": "3"}
```
生成见证：
```
node generate_witness.js multiplier2.wasm input.json witness.wtns
snarkjs wtns export json witness.wtns
```
`witness.json`的内容：
``` json
[
 "1",
 "6",
 "2",
 "3"
]
```
符合R1CS的预期布局[1, c, a, b]

## Circom 语法

### 信号（signal）& 变量（variable）

- 命名：字母 ( + `_` `$` ):
    ```
    signal input _in; 
    var o_u_t;
    var o$o;
    ```
- 信号是不可变的，旨在成为 R1CS 的列之一。变量不是 R1CS 的一部分。它用于在 R1CS 之外计算值，用来帮助定义 R1CS。
- `<--`、`<==`和`===`操作符用于信号，而不是变量。
在处理变量时，Circom 的行为类似于普通的 C 语言，如`=` `==` `>=` `<=` `!=` `++` `--`

### 关键字

```
signal input output public 
template component var function return 
if else for while do 
log assert include parallel
pragma circom
pragma custom_templates
bus
```

### `<--`、`<==`和`===`
`<==`操作符先计算，然后赋值，然后添加约束，等同于先`<--`后`===`

### 运算符
- `&&` `||` `!` `&` `|` `~` `^` `<<` `>>` 与C相同
- 三目运算符，不能嵌套：
    ```
    var z = x>y? x : y;
    ```
- `/`除 `\`取商 `%`取余

### 语句

```
var x = 0;
var y = 1;
if (x >= 0) {
   x = y + 1;
   y += 1;
} else {
   y = x;
}
```

```
var y = 0;
for(var i = 0; i < 100; i++){
    y++;
}
```

```
var y = 0;
var i = 0;
while(i < 100){
    i++;
    y += y;
}
```

### 数组

```
pragma circom 2.0.0;

template Powers(n) {
    signal input a;
    signal output powers[n];
    
    powers[0] <== a;
    for (var i = 1; i < n; i++) {
        powers[i] <==  powers[i - 1] * a;
    }
}
component main = Powers(6);
```

- 模板是使用 n 作为参数化的，即 Powers(n)
- 一个 R1CS 必须是固定且不可变的，这意味着一旦定义了行数或列数，我们就不能更改它们，也不能更改矩阵或证明的值。这就是为什么最后一行有一个硬编码的参数 Powers(6)，这个大小必须是固定的
- 然而，如果我们以后想要重用这段代码来支持不同大小的电路，那么让模板能够根据需要改变大小会更加方便。因此，组件可以接受参数来参数化控制流和数据结构，但这必须是每个电路固定的


### 总线 Bus

类似于结构体（2.2新增）
```
bus NameBus(param1,...,paramN){
    //signals, 
    //arrays,
    //other buses...
}
```












## Circom Compiler


