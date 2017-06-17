# 变量/常量

```rust
let x = 5;
```

x 是不可变的（`x = 6` 这会报错），可变的话加上`mut`:

```rust
let mut x = 5;
x = 10;
```

指定类型

```rust
let x: i32 = 5;
```

以 i 开头的代表有符号整数，而 u 开头的代表无符号整数。可能的整数大小是 8，16，32 和 64 位。



变量在使用前必须初始化，下面的代码运行会报错：

```rust
	let x: i32;
    println!("{}", x);
```



变量可以重复声明（类型可以不同，这意味着常量其实可以改变？）：

```rust
	let x = 5;
    println!("{}", x);

    let x = "hello";
    println!("{}", x);
```



将变量赋值给常量：

```rust
	let mut x = 5;
	let y = x;
	println!("{}", y)
```

## 类型

- `bool` : `true` | `false`
- `char` : `let c: char = '中';` `char` 代表unicode字符，占4个字节。
- 固定大小整型：有符号`i8`, 无符号 `u8`, 位数可以是8, 16, 32, 64
- 可变大小整型：`isize`, `usize`, 依赖底层机器指针大小。
- 浮点数：`f32`, `f64`  `let x: f64 = 3.14;`
- 数组：定长相同类型 `let arr: [i32; 3] = [1, 2, 3];`
- 切片
- 字符串
- 元组
- vector: `Vec<T>`
- 结构体 `struct`



### 数组

快速初始化：

```rust
let arr: [i32; 3] = [0; 3];
let arr = [0; 3];
```

下标访问：

```rust
arr[0]
```

长度：

```rust
arr.len()
```



### 切片

创建：

```rust
let arr = [0, 1, 2, 3];
let slice = &arr[1..3]; // 包括1不包括3，[..] 表示全部
```



### 元组

创建：

```rust
//    let tup: (i32, &str) = (3, "hello");
    let tup = (3, "hello");
    let (x, y) = tup; // 解构
    println!("{}, {}", x, y);
```

消除一个单元素元组和一个括号中的值的歧义：

```rust
(0,); // single-element tuple
(0); // zero in parentheses
```

通过索引访问：

```rust
    let tup = (3, "hello");
    let x = tup.0;
    let y = tup.1;
    println!("{}, {}", x, y);
```



### vectors

动态数组，总是在堆上分配数据。

```rust
    let v = vec![1, 2, 3];
    println!("v[1] = {}", v[1]);

    let v = vec![1; 10]; // 10个元素，初始化为1
    println!("v[1] = {}", v[1]);
```

索引的类型只能是 **usize**:

```rust
    let v = vec![1; 10]; // 10个元素，初始化为1
    let i: usize = 1; // 类型只能是usize
    println!("v[1] = {}", v[i]);
```

越界访问：

```rust
    let v = vec![1; 10]; // 10个元素，初始化为1
    match v.get(9) {
        Some(x) => println!("get {}", x),
        None => println!("nothing")
    }
```

遍历：

```rust
	for i in v {
        println!("get {}", i)
    }
    for i in &v {
        println!("get {}", i)
    }
    for i in &mut v {
        println!("get {}", i)
    }
```

# 结构体

```rust
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let p = Point { x: 1, y: 2 };
    println!("x = {}, y = {}", p.x, p.y);
}
```

修改字段值：

```rust
let mut p = Point { x: 1, y: 2 };
p.x = 3;
```

修改结构体的字段然后让其不可变：

```rust
	let mut p = Point { x: 1, y: 2 };
    p.x = 3;
    let p = p; // 现在p不再可变了
	// p.y = 4; // error
```

从另一个结构体复制字段的值：

```rust
struct Point {
    x: i32,
    y: i32,
    z: i32,
}

fn main() {
    let p1 = Point { x: 1, y: 2, z: 3 };
    let p2 = Point { x: 4, ..p1 }; // 从p1复制y和z的值
    println!("p2.x = {}, p2.y = {}, p2.z = {}", p2.x, p2.y, p2.z);
}
```

## 元组结构体

使用 `()` 而不是 `{}`, 字段没有名称。

```rust
struct Point(i32, i32, i32);

fn main() {
    let p = Point(1, 2, 3);
    let Point(x, y, z) = p; // 这样来获取结构体的字段值
    println!("x = {}, y = {}, z = {}", x, y, z);
}
```

当结构体只有一个字段时比较有用：

```rust
struct Point(i32);

fn main() {
    let p = Point(1);
    let Point(x) = p;
    println!("x = {}", x);
}
```

## 类单元结构体（没有字段）

```rust
struct Point;
let p = Point;
```

## 枚举

```rust
enum Message {
    Quit,
    ChangeColor(i32, i32, i32),
    Move {
        x: i32,
        y: i32
    },
    Write(String),
}

fn main() {
    // 创建
    let quit: Message = Message::Quit;
    let change_color: Message = Message::ChangeColor(1, 2, 3);
    let m = Message::Move { x: 1, y: 2 };
    let w = Message::Write("hello".to_string());

    // 匹配
    process_message(quit);
    process_message(change_color);
    process_message(m);
    process_message(w);
}

fn process_message(m: Message) {
    match m {
        Message::Quit => println!("quit message"),
        Message::ChangeColor(r, g, b) => println!("change color({}, {}, {})", r, g, b),
        Message::Move { x, y } => println!("move({}, {})", x, y),
        Message::Write(s) => println!("write {}", s),
    };
}
```



# 注释

- 行注释 `//`
- 文档注释 `///` 支持markdown语法



# if

```rust
    let x = 5;
    if x == 5 {
        println!("x == 5")
    } else if x == 6 {
        println!("x == 6")
    } else {
        println!("x == 7")
    }
```

三元运算：

```rust
    let x = 5;
    let y = if x == 5 { 10 } else { 11 };
    println!("{}", y)
```

# loop

无线循环

```rust
	loop {
        print!("loop forever");
    }
```

# while

```rust
    let mut i = 0;
    let mut done = false;
    while !done {
        println!("{}", i);
        i += 1;
        if i == 5 { done = true; }
    }
```

# for

```rust
	for i in 0..5 {
        println!("{}", i);
    }
```

抽象形式：

```rust
for var in expression {
  code
}
```

enumerate方法：

```rust
	for (i, v) in (5..10).enumerate() {
        println!("{}: {}", i, v);
    }
```

输出：

```
0: 5
1: 6
2: 7
3: 8
4: 9
```

标签：

```rust
    'outer: for x in 0..10 {
        'inner: for y in 0..10 {
            if x % 2 == 0 { continue 'outer; }
            if y % 2 == 0 { continue 'inner; }
            println!("x: {}, y: {}", x, y)
        }
    }
```

# match

```rust
    let x = 3;
    match x {
        1 => println!("one"),
        2 => println!("two"),
        3 => println!("three"),
        _ => println!("other"),
    };
```

match 是一个表达式：

```rust
    let x = 3;
    let y = match x {
        1 => "one",
        2 => "two",
        3 => "three",
        _ => "other",
    };
    println!("{}", y);
```

匹配多个模式：

```rust
	let x = 2;
    let y = match x {
        1 | 2 => "one or two",
        3 => "three",
        _ => "other",
    };
```

解构：

```rust
	let p = Point { x: 45, y: 90 };
    match p {
        Point { x, y } => println!("point({}, {})", x, y),
    }
```

使用其他名字解构：

```rust
	let p = Point { x: 45, y: 90 };
    match p {
        Point { x: x1, y: y1 } => println!("point({}, {})", x1, y1),
    }
```

只解构部分字段：

```rust
	let p = Point { x: 45, y: 90 };
    match p {
        Point { x, .. } => println!("point({} ..)", x),
    }
```



# 所有权

```rust
    let v = vec![1, 2, 3];
    let x = v;
    print!("x={}, v={}", x[0], v[0]); // error: use of moved value: `v`
```

v一旦赋给x后，就不能再使用v了，**所有权被移交了**，这种情况也适用于函数调用：

```rust
fn main() {
    let v = vec![1, 2, 3];
    foo(v);
    print!("v={}", v[0]); // error
}

fn foo(v: Vec<i32>) {}
```

`Vec` 在栈上和堆上都包含信息，拷贝时只会拷贝栈上的，如果x改变了vec，v将不会知道，所以不能继续使用v了。

基本类型不受这个限制，因为拷贝时是全部拷贝的（没有指向其他数据的指针，Copy trait）。

```rust
    let a = 1;
    let b = a;
    print!("a = {}, b = {}", a, b);
```

## 不可变引用

```rust
fn main() {
    let v = vec![1, 3];
    let r = f(&v);
    print!("r = {}, v[0] = {}", r, v[0]); // 继续使用v
}

fn f(v: &Vec<i32>) -> i32 {
//    v.push(3); // 这里不能修改v中的元素
    42
}

```

`v` 是数据的所有者，函数f的参数借用了v，借用后所有者就不能读写该数据了，避免悬空指针。

在租借期内，所有者必须保证不会释放/转移/租借该内存。

## 可变引用

```rust
fn main() {
    let mut v = vec![1, 3];
    let r = f(&mut v);
    print!("r = {}, v[0] = {}", r, v[2]); // 继续使用v
}

fn f(v: &mut Vec<i32>) -> i32 {
    v.push(3); // 这里可以修改v中的元素
    42
}

```

```rust
    let mut x = 5;
    {
        let y = &mut x;
        *y += 1; // 这里要加*
        // println!("x = {}", x); // 这里不能再使用x了,y是一个&mut T,不能再创建&T了。
    }
    println!("x = {}", x); // 6
```

## 规则

> 同一作用域下，要么只有一个对资源A的可变引用（&mut T），要么有N个不可变引用（&T），不能同时存在可变和不可变引用。

# 生命周期

租借期扩展到作用域外：

```rust
fn main() {
    let mut a = vec![1, 2, 3];
    let b: &Vec<i32>;
    if false {
        let c = &a; // 借给c了
        b = c; // 现在借给b了，这个作用域有两个不可变引用
        println!("c[0] = {}, b[0] = {}", c[0], b[0]);
    }
    // 现在这个作用域里存在了不可变引用b，所以a不能修改。
    // a[0] = 4;
    println!("{}", a[0]);
}
```

多个参数时，结果的生命周期要比所有的参数的都小：

```rust
fn main() {
    let a = 1;
    let c: &i32;
    {
        let b = 2;
        c = max(&a, &b); // error: `b` does not live long enough
    }
    println!("{}", c);
}

fn max<'a>(a: &'a i32, b: &'a i32) -> &'a i32 {
    if *a > *b {
        a
    } else {
        b
    }
}
```

上面c和a的生命周期都比b长，所以编译出错。