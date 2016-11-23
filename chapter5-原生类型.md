chapter5 原生类型
=================

#### 布尔型：

布尔类型（`bool`）只有两个值：`true` 和 `false`：

```rust
let x = true;
let y: bool = false;
```

布尔型通常用在 `if` 语句中, 也可以用在 `match` 语句中：

```rust
fn main() {
    let praise_the_borrow_checher = true;

    if praise_the_borrow_checher {
        println!("oh, yeah!");
    } else {
        println!("what?!!");
    }

    match praise_the_borrow_checher {
        true => println!("keep praising!"),
        false => println!("you should praise!"),
    }
}
```

还可以将字符串 “true” 和 “false” 转换为 `bool`:

```rust
use std::str::FromStr;

fn main() {
    assert_eq!(FromStr::from_str("true"), Ok(true));
    assert_eq!(FromStr::from_str("false"), Ok(false));
    assert!(<bool as FromStr>::from_str("not even a boolean").is_err());
}
```

也可以这样：

```rust
    assert_eq!("true".parse(), Ok(true));
    assert_eq!("false".parse(), Ok(false));
    assert!("not even a boolean".parse::<bool>().is_err());
```

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.bool.html)查看更多 `bool` 的说明。

#### char

`char` 类型代表一个单独的 Unicode 字符的值。可以使用单引号 `'` 创建 `char`:

```rust
let x = 'x';
let two_hearts = '💕';
```

与其他语言不同的是，Rust 的 `char` 并不是1个字节，而是4个。因而我们可以将各种奇怪的字符随心所欲的赋值给一个 `char` 类型。

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.char.html)查看更多 `char` 的说明。

#### 数字类型

数字类型分为有符号整数（`i8`, `i16`, `i32`, `i64`, `isize`）、无符号整数（`u8`, `u16`, `u32`, `u64`, `usize`）和浮点数（`f32`, `f64`）。其中，`isize` 和 `usize` 也称为可变大小类型或自适应类型，其大小依赖底层机器指针大小，意思就是：32位电脑上就是32位，64位电脑上就是64位。

如果一个数字，没有推断它类型的条件，将采用默认类型：

```rust
let x = 123; //i32
let y = 1.23; //f64;
let c = -456; //i32;
```

需要说明的是，这里的“没有推断它类型的条件” 中的 “条件”。比如：

```rust
fn main() {
    let a = 18446744073709551615;
    println!("{:?}", a);
}
```

Rust 不会根据赋给 `a` 的值推断 `a` 的类型，`a` 仍然是i32类型，把一个大于32位的值赋给i32类型，会警告越界：

```rust
   Compiling hello_world v0.1.0 (yourpath/hello_world)
warning: literal out of range for i32, #[warn(overflowing_literals)] on by default
 --> main.rs:2:13
  |
2 |     let a = 18446744073709551615;
  |             ^^^^^^^^^^^^^^^^^^^^

    Finished debug [unoptimized + debuginfo] target(s) in 0.47 secs
     Running `yourpath\hello_world\target\debug\hello_world.exe`
-1
```

即使能编译成功，也将变为-1。如果给定一个接收者，比如一个函数：

```rust
fn main() {
    let a = 18446744073709551615;
    print(a);
}

fn print(n: u64) {
    println!("{:?}", n);
}
```

这样，Rust 就能根据 `print` 函数所接受的 `u64` 类型推断出 `a` 的类型为 `u64`。

如果显示的标注类型，有两种方式：

```rust
let x: i32 = 123;
let y: f64 = 1.23;
```

或者通过后缀的方式：

```rust
let x = 123i32;
let y = 1.23f64;
```

以上两种方式是等价的。

为了可读性，可以在数字之间加上下划线 `_`：

```rust
let x = 1_000i32; // 1000
let y = 1_234_567; // 1234567
let z = 0.000_001; // 0.000001
```

如果你想的话，这样都可以：

```rust
let x = 1000_i32;
let y = 1.23_f64;
```

还可以加上前缀 `0x`、`0o`、`0b` 分别表示十六进制数、八进制数、二进制数：

```rust
let a = 0b11111111; //255
let b = 0o77777777; //16777215
let c = 0xFFFFFF;  //16777215
```

可以标准库文档中查看每种类型的详细说明：[i8](https://doc.rust-lang.org/std/primitive.i8.html)，[i16](https://doc.rust-lang.org/std/primitive.i16.html)，[i32](https://doc.rust-lang.org/std/primitive.i32.html)，[i64](https://doc.rust-lang.org/std/primitive.i64.html)，[u8](https://doc.rust-lang.org/std/primitive.u8.html)，[u16](https://doc.rust-lang.org/std/primitive.u16.html)，[u32](https://doc.rust-lang.org/std/primitive.u32.html)，[u64](https://doc.rust-lang.org/std/primitive.u64.html)，[isize](https://doc.rust-lang.org/std/primitive.isize.html)，[usize](https://doc.rust-lang.org/std/primitive.usize.html)，[f32](https://doc.rust-lang.org/std/primitive.f32.html)，[f64](https://doc.rust-lang.org/std/primitive.f64.html)。

#### 数组(array)

数组是一组包含相同数据类型 `T` 的组合，并存储在连续的内存区中。数组使用中括号 `[]` 来创建，其大小在编译期间就已确定，数组的类型被标记为 `[T; size]`，表示一个拥有 `T` 类型，`size` 个元素的数组。数组的大小是固定的，但其中的元素可以被更改。

```rust
fn main() {
    let mut array: [i32; 3] = [0; 3]; // 0, 0, 0

    array[1] = 1;
    array[2] = 2;

    println!("{:?}", array); // 0, 1, 2
}
```

可以用 `array.len()` 获取数组的元素数量：

```rust
fn main() {
    let array = [0; 20];
    println!("{:?}", array.len()); // 20
}
```

可以使用下标访问特定元素：

```rust
fn main() {
    let names = ["Graydon", "Brian", "Niko"]; // names: [&str, 3]
    println!("The second name is: {}", names[1]); // Brian
}
```

跟大部分编程语言一样，下标从0开始，所以第一个元素是 `names[0]`, 第二个是 `names[1`。如果你尝试使用一个不存在于数组中的下标，将会得到一个错误，数组访问会在运行时进行边界检查。

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.array.html)中查看更多 `array` 的详细说明。

#### 切片（slice）

切片是一个数组的引用（或者“视图”）。它有利于安全，有效的访问数组的一部分而不用进行拷贝。比如，你可能只想要引用读入到内存文件中的一行。原理上，切片并不是直接创建的，而是引用一个已经存在的变量。片段有预定义长度，可以是可变的也可以是不可变的，但是其范围不能超过数组的大小。

在底层，切片代表一个指向数据开始的指针和一个长度。

一个切片的表达式可以为 `[T]` 或 `&mut [T]`。

```rust
let array = [1, 2, 3, 4, 5, 6];
let slice_complete = &array[..]; // 获取全部元素
let slice_middle = &array[1..4]; // 获取中间元素，取得的 Slice 为 [2, 3, 4]。切片遵循左闭右开原则。
let slice_right = &array[1..]; // [2, 3, 4, 5, 6]
let slice_left = &array[..3]; // [1, 2, 3]
```

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.slice.html)中查看更多切片的说明。

#### 元组（tuples）

元组是固定大小的有序列表，使用括号 `()` 来构成，每个元组的值都是 `(T1, T2, ...)`类型标记的形式，其中 `T1, T2` 是每个元素的类型。 如下：

```rust
let x = (1, "hello", "world");
```

这是一个长度为3的元组，下面是同样的元组，不过注明了数据类型：

```rust
let x: (i32, &str, &str) = (1, "hello", "world");
```

如你所见，元组的类型跟元组看起来很像，只不过类型取代值的位置。

可以把一个元组赋值给另一个，如果他们包含相同的类型和数量。当元组有相同的长度时他们后相同的数量。

```rust
let mut x = (1, 2); // x: (i32, i32)
let y = (2, 3); // y: (i32, i32)

x = y;
```

可以通过一个`let` “解构”访问元组中的字段：

```rust
let (x, y, z) = (1, 2, 3);

println!("x is {}", x);
```

我们可以在 `let` 左侧写一个模式，如果能匹配右侧的话，可以一次写多个绑定。这种情况下，`let` “解构” 或 “拆开” 了元组，并分成了三个绑定。这个模式时很强大的。

可以用一个逗号来消除一个单元素元组和一个括号中的值的歧义：

```rust
(0, );
(0);
```

可以用索引访问一个元组中的字段：

```rust
let tuple = (1, 2, 3);

let x = tuple.0; // 1
let y = tuple.1; // 2
let x = tuple.2; // 3
```

就像数组索引，元组索引从 `0` 开始，不过也不像数组索引，它使用 `.`, 而不是 `[]`。

函数可以利用元组来放回多个值，因为元组可以拥有任意数量、不同类型的值。

```rust
fn mian() {
    let (p2, p3) = pow_2_3(456);
    println!("pow 2 of 456 is {}.", p2);
    println!("pow 3 of 456 is {}.", p3);
}

fn pow_2_3(n: i32) -> (i32, i32) {
    (n * n, n * n * n)
}
```

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.tuple.html)中查看更多元组的说明。

#### str

Rust 的 `str` 类型是最原始的字符串类型。作为一个不定长类型，它本身并不是非常有用，不过当它在引用后就是有用的了，例如 `&str`。事实上，Rust 中所有用 `""` 包裹起来的都可以称为 `&str`，但是这个类型单独使用的情况很少。将在后续章节详细介绍字符串类型。

可以在[标准库文档](https://doc.rust-lang.org/std/primitive.str.html)中查看更多 `str` 的说明。

#### 函数

函数也有一个类型，他们看起来像这样：

```rust
fn foo(x: i32) -> i32 { x }

let x: fn(i32) -> i32 = foo;
```

在这个例子中，`x` 是一个“函数指针”，指向一个获取一个 `i32` 参数并返回一个 `i32` 值的函数。










