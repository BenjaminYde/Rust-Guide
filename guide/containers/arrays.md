# Creating and Using Arrays

## Definition and Initialization

In Rust, an **array** is a collection of elements of the same type, stored in contiguous memory with a fixed size known at compile time. You declare an array by specifying its type and size.

```rust
// Declares an array of 5 32-bit integers
let my_array: [i32; 5] = [10, 20, 30, 40, 50];
```

The type annotation `[i32; 5]` means "an array of `i32` elements with a length of 5". The compiler can often infer the type and size, so you can also write:

```rust
// The compiler infers the type as [i32; 5]
let my_array = [10, 20, 30, 40, 50];
```

You can also initialize an array with the same value for every element.

```rust
// Initializes an array of 10 elements, all with the value 0
let ten_zeros = [0; 10];
```

## Accessing Elements

Elements in a Rust array are accessed using an index, starting from 0.

```rust
let my_array = [10, 20, 30, 40, 50];

// Accessing the first element
let first_value = my_array[0]; // first_value is 10

// Changing the value of the fourth element requires the array to be mutable
let mut another_array = [0; 4];
another_array[3] = 100;
```

## Size

You can get the number of elements in an array using the `.len()` method. This is a built-in feature and does not require calculations like C++'s `sizeof`.

```rust
let my_array = [10, 20, 30, 40, 50];
let size = my_array.len(); // size is 5
```

## Multidimensional Arrays

Rust supports multidimensional arrays by nesting the array types.

```rust
// A 2x3 array (2 arrays, each with 3 elements)
let matrix: [[i32; 3]; 2] = [
    [1, 2, 3],
    [4, 5, 6]
];

// Accessing an element
let value = matrix[1][2]; // value is 6
```

## Example of Using an Array

Here's a complete example of declaring, initializing, and iterating over an array. The idiomatic way to loop over an array in Rust is with a `for` loop, which iterates through each element.

```rust
fn main() {
    let numbers = [10, 20, 30, 40, 50];

    println!("Array elements are: ");
    for number in numbers {
        print!("{} ", number);
    }
    println!(); // For a final newline

    println!("The array has {} elements.", numbers.len());
}
```

Yes, there are a few more details about arrays worth knowing.


## Array Slices

While you've seen that `&[i32]` can be used for function parameters, this is a more general type called a "slice". A **slice** provides a view into a contiguous sequence of elements in a collection, rather than the whole collection. Slices are a type of reference, so they don't have ownership.

You can create a slice from an array by specifying a range.

```rust
fn main() {
    let numbers: [i32; 5] = [10, 20, 30, 40, 50];

    // A slice of the entire array.
    let full_slice: &[i32] = &numbers;

    // A slice of part of the array.
    // This includes elements at index 1 and 2.
    let partial_slice: &[i32] = &numbers[1..3];

    println!("The partial slice is: {:?}", partial_slice); // Prints [20, 30]
}
```

## Array Iterators

The `for` loop is the most common way to go through an array's elements. When you use a `for` loop like `for item in my_array`, you are implicitly creating an iterator. Rust provides several explicit iterator methods for more control.

- **.iter()**: Iterates over references to the elements (`&T`). This is the most common one and is what a `for` loop uses by default.
- **.iter\_mut()**: Iterates over mutable references (`&mut T`), allowing you to modify the elements.
- **.into\_iter()**: Iterates over the elements by value (`T`), consuming the array in the process. After this, the array can no longer be used.

```rust
fn main() {
    let mut numbers = [1, 2, 3];

    // Using .iter_mut() to modify each element.
    for number_ref in numbers.iter_mut() {
        // `number_ref` is a mutable reference, so we dereference it to change the value.
        *number_ref *= 2;
    }

    println!("The modified array is: {:?}", numbers); // Prints [2, 4, 6]
}
```

## Caveats and Considerations

### Bounds Checking ✅

Unlike C++, Rust **always** performs bounds checking on array access. If you try to access an element with an index that is out of bounds, the program will **panic** (a controlled crash) and shut down. This prevents undefined behavior and is a core memory safety feature of Rust.

### Stack Allocation

Like in C++, arrays in Rust are allocated on the stack. For a dynamic, growable collection, you should use a `Vec<T>` (a vector), which is allocated on the heap and can change in size.

### Passing Arrays to Functions

When you pass an array to a function, the behavior is different from C++.

- By default, Rust passes the **entire array by value** (a full copy), if the elements are copyable. This can be inefficient for large arrays.
- The idiomatic way to pass an array is by using a **slice**. A slice is a "view" or "reference" to a portion of an array (or other collection) that does not have ownership. It retains size information and is very efficient.

```rust
fn main() {
    let my_array = [1, 2, 3, 4, 5];
    
    // The `&` creates a slice that refers to the entire array.
    print_slice(&my_array);
}

// This function takes a slice of 32-bit integers.
// It can accept a reference to an array, a vector, or part of either.
fn print_slice(arr_slice: &[i32]) {
    println!("\nSlice has {} elements.", arr_slice.len());
    for element in arr_slice {
        print!("{} ", element);
    }
}
```