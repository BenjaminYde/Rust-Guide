# Control Flow

## Conditional Logic with `if` Expressions

The most basic way to make a decision is with an `if`/`else` block. The syntax is straightforward: the condition does **not** need to be wrapped in parentheses, and it **must** evaluate to a `bool`.

```rust
fn main() {
    let number = 7;

    if number < 10 {
        println!("Condition was true: number is less than 10.");
    } else {
        println!("Condition was false: number is not less than 10.");
    }
    
    // You can also use `else if` for multiple conditions.
    let another_number = 6;

    if another_number % 4 == 0 {
        println!("Number is divisible by 4");
    } else if another_number % 3 == 0 {
        println!("Number is divisible by 3");
    } else {
        println!("Number is not divisible by 4 or 3");
    }
}
```

### The "Rust" Way: `if` is an Expression

In Rust, `if` is an **expression**, not just a statement. This means it can evaluate to a value, which you can then assign to a variable using `let`.

This is a common pattern for initializing a variable based on a condition.

```rust
fn main() {
    let is_logged_in = true;
    
    // Use `if` as an expression to assign a value to `status`.
    let status = if is_logged_in { "Online" } else { "Offline" };

    println!("User status: {}", status); // Prints "User status: Online"
}
```

**How Not To Do It (Type Mismatch)**

Because `if` is an expression that produces a single value, both the `if` and `else` blocks must return a value of the **same type**. If they don't, the compiler will throw an error.

```rust
// THIS WILL NOT COMPILE!
fn main() {
    let condition = true;

    // Error: `if` and `else` have incompatible types.
    // The `if` block returns a string literal `&str`, but the `else` block returns an integer `i32`.
    let number = if condition { "five" } else { 5 }; 
}
```

### Pattern Matching with `match`

While `if` is good for a simple true/false decision, Rust has a better tool for handling multiple possible conditions: the `match` statement. Think of it as a `switch` statement on steroids. It compares a value against a series of patterns and executes the code for the first pattern that matches.

The most important feature of `match` is that it is **exhaustive**. This means you **must** handle every possible value the type can have. The compiler guarantees this, which eliminates a whole class of common bugs.

```rust
enum TrafficLight {
    Red,
    Yellow,
    Green,
}

fn main() {
    let light = TrafficLight::Green;

    match light {
        TrafficLight::Red => println!("Stop!"),
        TrafficLight::Yellow => println!("Slow down."),
        TrafficLight::Green => println!("Go!"),
    };
    // If you were to comment out one of the arms, the code would not compile!
}
```

#### The Catch-All `_` Pattern

If you want to handle a few specific cases but don't care about the rest, you can use the underscore `_` as a catch-all pattern. It will match any value but will not bind it to a variable. This is how you satisfy the exhaustiveness check without listing every possibility.

```rust
fn main() {
    let number = 13;

    match number {
        1 => println!("One!"),
        2 | 3 | 5 | 7 | 11 => println!("This is a prime"),
        _ => println!("It's something else."), // Handles all other numbers
    }
}
```

## Repetition with Loops

Rust provides three ways to loop: `loop`, `while`, and `for`.

### `loop`: The Infinite Loop

The `loop` keyword creates a loop that will run forever until you explicitly tell it to stop using the `break` keyword.

```rust
fn main() {
    let mut counter = 0;
    println!("Starting infinite loop...");

    loop {
        counter += 1;
        println!("Again!");

        if counter == 3 {
            break; // Stop the loop
        }
    }
    println!("Loop finished.");
}
```

Like `if`, a `loop` can also be an expression that returns a value. You can pass a value back from the loop with the `break` keyword, and then assign it to a variable.

```rust
fn main() {
    let mut counter = 0;

    let result = loop {
        counter += 1;

        if counter == 10 {
            // Stop the loop AND return the value `counter * 2`.
            break counter * 2;
        }
    };

    println!("The result from the loop is: {}", result); // Prints 20
}
```

### `while`: The Conditional Loop

A `while` loop runs as long as its condition remains `true`. This is useful when you don't know the exact number of iterations ahead of time.

```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{}!", number);
        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

### `for`: The Idiomatic Iterator Loop

The `for` loop is the most common and idiomatic way to loop in Rust. It is used to iterate over a collection, such as an array, a vector, or a range. It's safer and cleaner than using a `while` loop with a manual index.

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    // `for` loop to iterate over each element in the array `a`.
    for element in a {
        println!("The value is: {}", element);
    }
    
    // You can also use a range to execute a block of code a certain number of times.
    // `1..4` creates a range from 1 up to (but not including) 4.
    for number in 1..4 {
        println!("{}!", number);
    }
    // To include the final number, use `..=`
    // for number in 1..=3 { ... }
}
```

**Pros of `for` loops:**

  * **Safety:** You can't accidentally go out of bounds with an incorrect index.
  * **Clarity:** The intent to iterate over every item in a collection is very clear.

## Advanced Control Flow

### `if let`: Concise Pattern Matching

You can use `if let` to check if a value has a certain structure and grab the pieces you need at the same time. For example, imagine you have a tuple `(x, y)` and only want to do something if y is 0.

```rust
fn main() {
    let point = (5, 0);

    // This code asks: "Does `point` match the pattern of `(some_number, 0)`?"
    // If YES, it copies `some_number` into the new variable `x`
    // and runs the code inside the braces.
    if let (x, 0) = point {
        println!("The point is on the x-axis at position {}!", x);
    }
    
    // If the point was (5, 3), the pattern wouldn't match, and the code inside
    // the `if let` would be skipped.
}
```

### Loop Labels

If you have nested loops, `break` and `continue` apply to the innermost loop by default. To control an outer loop, you can use **loop labels**. A label is a name you give a loop, starting with a single quote.

This allows you to `break` or `continue` a specific, named outer loop from an inner loop.

```rust
fn main() {
    'outer_loop: loop {
        println!("Entered the outer loop");
        let mut inner_counter = 0;

        'inner_loop: loop {
            println!("Entered the inner loop");
            
            if inner_counter > 2 {
                // This would only break the inner loop.
                // break; 
            }
            
            if inner_counter > 1 {
                // This breaks the specific outer loop.
                println!("Breaking outer loop from inner loop!");
                break 'outer_loop;
            }
            inner_counter += 1;
        }
        // This line is never reached because we break 'outer_loop.
        println!("This will not be printed");
    }
    println!("Exited outer loop");
}
```