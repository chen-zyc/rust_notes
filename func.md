# 声明

```rust
fn main() {
	hello();
	sum(1, 2);
	println!("incr 7 = {}", incr(7));
}

// 无参无返回值
fn hello() {
	println!("hello");
}

// 有参无返回值
fn sum(a: i32, b: i32) {
	println!("sum({}, {}) = {}", a, b, a + b);
}

// 有参有返回值
fn incr(i: i32) -> i32 {
	// return i + 1
	i + 1 // 还可以这样写，注意末尾不能有分号
}

// 多个返回值
fn f() -> (i32, i32) {
    (1, 2)
}
// 使用:let (a, b) = f();
```



# 函数指针

就是将函数的指针赋给常量

```rust
fn main() {
	let f: fn(i: i32)->i32 = incr; // lef 常量名: 类型 = 函数指针, 可以不指定类型：let f = incr;
	f(4);
}

fn incr(i: i32) -> i32 {
	// return i + 1
	i + 1 // 还可以这样写，注意末尾不能有分号
}
```

