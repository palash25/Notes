## Installation

Rust can be installed through `rusetup` a CLI to manage Rust versions and
associated tools.

To install rust download and run the following shell script:

```
$ curl https://sh.rustup.rs -sSf | sh
```

## Update

```
$ rustup update
```

## Uninstall

```
$ rustup self uninstall
```
### Hello World

Write a file `main.rs` for word separators in filenames use underscores.

```rust
fn main() {
    println!("Hello world");
}
```

**Compile and Run**

```
$ rustc main.rs
$ ./main
```
Rust is an ahead-of-time compiled language, which means that one can compile a
program, give the executable to someone else, and they can run it even without
having Rust installed.

## Rust Program: Anatomy

- The main function is the first one to run for every executable rust program.
- Indentation uses 4 spaces (not tabs).
- Function calls with `!` at the end like the `println!` in the above example
  means its calling a Rust macro. Normal function calls don't include `!`.
- Semicolons are used to indicate end of expressions.


## Cargo

Cargo is used for the following tasks in rust projects:
- as a package manager
- as a build system
- downloads and builds the project dependencies

### Creating a project with Cargo

```
$ cargo new hello_cargo --bin
$ cd hello_cargo
```

This creates a new executable in the current directory from which the command
was run from named `hello_cargo`. 
