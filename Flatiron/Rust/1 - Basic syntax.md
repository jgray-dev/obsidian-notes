
functions are declared using the `fn` keyword. All lines must end with a semi colon <3

```
fn main() {
	println!("Hello world!");
}
```


To declare the return type of a function, this syntax is used

```
fn multiply(a: i32, b: i32) -> i32 {
    return a * b
}
```

Inside the parenthesis, `a: i32` and `b: i32` are our arguments. We must tell rust what type of arguments to expect, so replace `i32` with the expected type. We then use an arrow `->` to declare the return type. In this case its also an `i32`

The full usage for this multiple function looks like this:

```
fn main() {
    let x: i32 = 10;
    let y: i32 = 3;
    let product = multiply(x, y);
    println!("The product is {}", product)
    
}


fn multiply(a: i32, b: i32) -> i32 {
    return a * b
}
```








List of Data types