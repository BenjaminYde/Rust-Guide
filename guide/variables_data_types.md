# Rust Variables, Data Types and Type Inference

This section covers the foundational elements of Rust's syntax, how to work with variables, the built-in data types, and Rust's powerful type inference system.

## Basic Syntax Structure

Rust's syntax is designed for clarity and safety. Similar to languages like C++ or C#, it uses curly braces `{}` to define code blocks (like function bodies or if statements) and semicolons `;` to end most statements.

```rust
// This is a single-line comment in Rust.

/*
This is a multi-line comment.
It can span across several lines.
*/

// The `fn` keyword is used to declare a function.
// `main` is a special function that is the entry point of every Rust executable program.
fn main() {
    // Code inside the function body goes here.
    // Each action or instruction is a statement, typically ending with a semicolon.
    println!("Hello, world!"); // `println!` is a macro that prints text to the console.
}
```

**Key takeaways:**

- **`fn` keyword:** Declares a function. `main` is the entry point.
- **Code Blocks**: Defined by curly braces {}.
- **Comments:** `//` for single-line, `/* ... */` for multi-line.
  - **Statements** perform an action and do not return a value (e.g., `let x = 5;`). They end with a semicolon.

## Variables and Mutability

In Rust, variables are declared using the `let` keyword. This keyword binds a name to a value.

```rust
fn main() {
    // Declare a variable named 'x' and assign it the value 5.
    let x = 5;
    println!("The value of x is: {}", x);
}
```

By default, variables in Rust are **immutable**. This means that once you've assigned a value to a let variable, you cannot change that value later. This is a core design principle of Rust that helps prevent bugs and makes code safer, especially when dealing with concurrency.

If you try to change an immutable variable, the Rust compiler will give you an error:

```rust
fn main() {
    let x = 5;
    println!("The value of x is: {}", x);

    // x = 6; // If you uncomment this line, it will cause a compile-time error!
    // Error message: "cannot assign twice to immutable variable `x`"
}
```

### Making Variables Mutable

What if you do need a variable whose value can change? Rust allows this, but you must explicitly opt-in to mutability using the `mut` keyword when you declare the variable.

```rust
fn main() {
    // Declare a mutable variable named 'y' and assign it the value 10.
    let mut y = 10;
    println!("The initial value of y is: {}", y);

    // Now, we can change the value of 'y' because it's mutable.
    y = 20; // This is allowed
    println!("The new value of y is: {}", y);
}
```

**Can you make a variable mutable after declaration?** No. The `mut` keyword must be present at the point of declaration (`let mut y = 10;`). You cannot declare an immutable variable and then later decide to make it mutable. If you need to change its value, it must be declared with `mut` from the start.


**Pros of Immutability by Default:**

  * **Safety:** Reduces the chances of unexpected side effects and bugs, especially in concurrent programming.
  * **Readability:** Makes it easier to reason about code as values don't change unexpectedly.
  * **Performance:** The compiler can make more optimizations knowing values won't change.

**Cons of Immutability by Default:**

  * **Verbosity:** Requires `mut` keyword for every variable that needs to change, which can feel verbose initially.

## Data Types

Rust is a **statically-typed** language. This means that the type of every variable must be known at compile time. However, as we'll see with type inference, you don't always have to write it out explicitly.

Rust has several primitive data types:

### Integers

**Integers**: Used for whole numbers. Rust provides various sizes, both signed (can be positive or negative) and unsigned (only positive or zero).

- **Signed Integers (start with `i`)**: i8, i16, i32 (default), i64, i128, isize.
- **Unsigned Integers (start with `u`)**: u8, u16, u32 (default), u64, u128, usize.
- `isize` and `usize` are pointer-sized integers, meaning their size depends on the architecture of your computer (e.g., 64-bit on a 64-bit system). They are primarily used for indexing into collections.

```rust
fn main() {
    let decimal = 98_222; // Inferred as `i32` by default. Underscores improve readability.
    let hex = 0xff;       // Hexadecimal
    let octal = 0o77;     // Octal
    let binary = 0b1111_0000; // Binary
    let byte = b'A';      // `u8` only, represents the ASCII value of 'A'

    // You can explicitly annotate the type if inference isn't desired or possible:
    let a: i32 = 42; // 32-bit signed integer
    let b: u32 = 10; // 32-bit unsigned integer
    let large_number: u64 = 5_000_000_000;
    let small_byte = 255u8; // You can also use a type suffix

    println!("Decimal: {}", decimal);
    println!("Hex: {}", hex);
    println!("Octal: {}", octal);
    println!("Binary: {}", binary);
    println!("Byte ('A' ASCII value): {}", byte);
    println!("Large number (u64): {}", large_number);
    println!("Small byte (u8): {}", small_byte);
}
```

### Floating-Point Numbers

Used for numbers with decimal points.

- `f32` (single-precision)
- `f64` (double-precision) (default in Rust)

```rust
let c: f32 = 3.14; // 32-bit floating point
let d: f64 = 2.718; // 64-bit floating point
```

### Booleans

Represent truth values.

- `bool` - can be `true` or `false`.

```rust
fn main() {
    let is_active = true;
    let game_over: bool = false; // Explicit annotation
    println!("Is active? {}, Game over? {}", is_active, game_over);
}
```

### Characters

Represent a single Unicode scalar value.

- `char` - Four bytes in size, allowing it to represent a wide range of characters including emojis.

```rust
fn main() {
    let c = 'z';
    let z = 'ℤ';        // A mathematical symbol
    let happy_emoji = '😊'; // An emoji!
    println!("c: {}, z: {}, happy_emoji: {}", c, z, happy_emoji);
}
```

### Compound Types

Compound types can group multiple values into one type.

#### Tuples

- A fixed-size collection that can hold values of different types.
- Once declared, a tuple's size cannot change.
- Useful for grouping a few related values.

```rust
fn main() {
    // A tuple with an integer, a float, and a character.
    let person_data: (i32, f64, char) = (30, 1.75, 'M');
    println!("Person data: {:?}", person_data); // `{:?}` is used for debug printing compound types.

    // You can access elements by their index (starting from 0).
    let age = person_data.0;
    let height = person_data.1;
    let gender = person_data.2;
    println!("Age: {}, Height: {}, Gender: {}", age, height, gender);

    // You can also "destructure" a tuple into individual variables:
    let (my_age, my_height, my_gender) = person_data;
    println!("My age: {}", my_age);
}
```

- **Pros**: Simple way to return multiple values from a function.
- **Cons**: Can become less readable for many elements; structs are better for named fields.

#### Arrays

- A fixed-size collection that can hold values of the same type.
- Arrays are allocated on the stack.
- Their size is known at compile time and cannot change during runtime.

```rust
fn main() {
    let numbers = [1, 2, 3, 4, 5]; // Inferred type `[i32; 5]` (array of 5 i32s)
    let months: [&str; 3] = ["Jan", "Feb", "Mar"]; // Explicitly a 3-element array of string slices

    // You can also create an array by repeating a value:
    let fives = [5; 3]; // Creates `[5, 5, 5]`

    println!("First number: {}", numbers[0]);
    println!("All months: {:?}", months);
    println!("Fives array: {:?}", fives);

    // Accessing an element outside the array's bounds will cause a *runtime panic*.
    // This is Rust's way of ensuring memory safety even with arrays.
    // let out_of_bounds = numbers[10]; // Uncommenting this will cause a panic!
}
```

- **Pros**: Fixed-size, stack-allocated, useful for small, known-size collections.
- **Cons**: Cannot grow or shrink; for dynamic collections, use `Vec<T>` (Vectors, which we'll cover later).

### Type Inference

Rust's compiler is smart! It often doesn't require you to explicitly write out the type of every variable. This is called **type inference**. Based on the value you assign and how you use the variable, the compiler can usually figure out the correct type for you.

```rust
fn main() {
    let price = 100; // Rust infers `i32` because it's a whole number.
    println!("Price (inferred i32): {}", price);

    let temperature = 25.5; // Rust infers `f64` because it has a decimal point.
    println!("Temperature (inferred f64): {}", temperature);

    let greeting = "Hello!"; // Rust infers `&str` (a string slice).
    println!("Greeting (inferred &str): {}", greeting);

    let is_logged_in = false; // Rust infers `bool`.
    println!("Is logged in? (inferred bool): {}", is_logged_in);

    // If you need to specify a different type than the default inference,
    // or if the context is ambiguous, you add a type annotation.
    let small_num: u8 = 10; // Explicitly `u8`
    println!("Small number (explicit u8): {}", small_num);

    // A common scenario where explicit annotation helps inference:
    // The `parse()` method can convert a string to many numeric types.
    // Without `: i32`, Rust wouldn't know which integer type you want.
    let parsed_integer: i32 = "123".parse().expect("Failed to parse number!");
    println!("Parsed integer (explicit i32): {}", parsed_integer);
}
```

#### When Type Inference is Most Helpful:

- For simple literals (numbers, booleans, string slices).
- When the type is clear from the context of how the variable is used.

Sometimes, even if inference is possible, an explicit annotation can make the code clearer for human readers.

### Constants

Constants are values that are locked in for the entire life of the program. They aren't just immutable; they are truly constant.

You declare them with the `const` keyword.

```rust
const MY_COOL_CONST: u32 = 3; // Note the uppercase convention for the name.

fn main() {
    println!("The value is {}.", MY_COOL_CONST);
}
```

#### Why Use `const` When `let` is Already Immutable?

The difference is not about mutability, but about **when the value is determined**.

- A `let` variable's value, even if immutable, can be the result of a calculation, a function call, or anything else that is determined at runtime (when the program is running).
- A `const` value must be known at compile time (before the program even runs). You cannot use the result of a function call or any other runtime value for a constant.

#### Example

```rust
fn get_runtime_value() -> u32 {
    // Imagine some complex logic here that depends on user input or a file.
    // The result can't be known before running the program.
    return 5 
}

fn main() {
    // VALID: `max_size` is an immutable variable, but its value is
    // determined by a function call when this line is executed.
    let max_size = get_runtime_value();

    // INVALID: This will not compile! The compiler needs to know the exact
    // value of `MAX_SIZE` right now, but `get_runtime_value()` only runs later.
    const MAX_SIZE: u32 = get_runtime_value(); 
}
```

### Shadowing

Shadowing is when you declare a new variable with the same name as a previous variable. The new variable "shadows" the old one, meaning the old variable is inaccessible from that point forward within the same scope. You do this using the `let` keyword again.

```rust
fn main() {
    let x = 5;

    // Here, we enter a new inner scope
    {
        // This 'x' shadows the outer 'x'. It's a completely new variable.
        let x = x * 2; 
        println!("The value of x in the inner scope is: {}", x); // Prints 10
    }

    // Back in the outer scope, the shadowing ends.
    // The original 'x' is visible again.
    println!("The value of x in the outer scope is: {}", x); // Prints 5
}
```

#### Shadowing vs. Mutability (`mut`)

- **Mutating**: `let mut x = 5; x = 10;`
  - You are changing the value inside the same variable. The type must remain the same.
- **Shadowing**: `let x = 5; let x = "ten";`
  - This is shadowing. You are creating a brand new variable, also named x, that replaces the old one.
  - Crucially, you can change the type of the variable when you shadow.

```rust
// Without shadowing
fn main() {
    let spaces_str = "   ";
    let spaces_num = spaces_str.len();

    println!("Found {} spaces.", spaces_num);
}
```

```rust
// WITH shadowing - you can reuse the name, which is often cleaner.
fn main() {
    let spaces = "   "; // spaces is a &str
    let spaces = spaces.len(); // spaces is now shadowed and is a usize

    println!("Found {} spaces.", spaces);
}
```