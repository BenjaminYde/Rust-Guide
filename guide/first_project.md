# First Rust Project

## Creating a Basic Rust Project

Rust projects are managed by Cargo. It handles everything from creating the project structure to downloading dependencies and building your code.
To create a new project, open your terminal and run the following command:

```sh
cargo new hello_world
# output:
# Creating binary (application) `hello_world` package
# note: see more `Cargo.toml` keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
```

## What a Rust Project Looks Like

Let's examine the files Cargo generated for us. The `hello_world` directory contains two main items:

```
.
├── Cargo.toml
└── src
    └── main.rs
```

### `Cargo.toml`

This file is the manifest file for your Rust project. It's written in the TOML (Tom's Obvious, Minimal Language) format. This file is where you keep metadata for your project, as well as its dependencies (called "crates" in Rust)

Here is what the contents of `Cargo.toml` look like:

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2024"

[dependencies]
```

- `[package]`: Contains information about your project, like its name, version, and the Rust edition it uses.
- `[dependencies]`: This is where you would list any external Rust libraries (crates) your project needs. Our "Hello, world!" program doesn't have any, so this section is empty.

### `src/main.rs`

The `src` directory is where all your source code lives. By default, Cargo creates a `main.rs` file for a binary application. This is the root file of your program and the entry point for its execution.

```rs
fn main() {
    println!("Hello, world!");
}
```

## How to Compile & Run Your Project

Now that we have a project, let's compile and run it. Cargo makes this incredibly simple.

### The Easy Way: `cargo run`

The most common command you'll use during development is `cargo run`. This command handles both compiling and running the program in one step.
From your hello_world directory, execute:

```sh
cargo run
# output:
# Compiling hello_world v0.1.0 (/home/developer/Rust-Guide/examples/hello_world)
# Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.10s
# Running `target/debug/hello_world`
# Hello, world!
```

### The Step-by-Step Way: `cargo build`

To only compile your code, use `cargo build`. By default, this creates a debug build.

```sh
cargo build
# output:
# Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.00s
```

This command compiles your code. It creates a new directory called target where it places the compiled files. For a standard build, the executable will be located at `target/debug/hello_world`.

You can now run the program yourself by executing `./target/debug/hello_world`

To create a release build you can run `cargo build --release` which will create an excecutable in the `target/release` directory.

### Checking Your Code: `cargo check`

For larger projects, compiling can be slow. If you just want to check your code for errors without producing an executable, you can use `cargo check`. This is much faster than a full build.

## Syntax