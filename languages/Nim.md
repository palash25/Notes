# Nim

Notes from "Nim in Action".

## Tooling related features
- Nim Supports cross-compilation
- Supports different GC modes (ref counting, a custom GC, and Boehm) and also an option to disable the GC

## Language related features
- `main` functions are implicit in Nim, even if you don't provide one the compiler assumes one to be there.
- `procedure`s unlike methods found in mainstream langauges are not bound to an object/type in Nim but can still be called on it
```Nim

type
  Dog = object
  
proc bark(self: Dog) =
  echo("Woof!")

let dog = Dog()
dog.bark()
```
> Reason:  Nim rewrites `dog.bark()` to `bark(dog)`, making it possible for you to call methods using the traditional OOP style without having to explicitly bind them to a class. This is called Uniform Function Call Syntax and allows one to define new procedures on existing objects and chain function calls
- You can also do some FP-style coding usig closures
```Nim
import sequtils, future, strutils
let list = @["Dominik Picheta", "Andreas Rumpf", "Desmond Hume"]
list.map(
(x: string) -> (string, string) => (x.split[0], x.split[1])
).echo
```
- Nim implements an exception-tracking mechanism that is entirely opt-in. With exception tracking, you can ensure that a procedure
won’t raise any exceptions, or that it will only raise exceptions from a predefined list.
This prevents unexpected crashes by ensuring that you handle exceptions.
- type names start with an uppercase letter. Built-in types don’t follow
this convention,
- Comments and disabling code:
```Nim

# Single-line comment
#[
Multiline comment
]#
# disables code, parsed by the compiler but not executed
when false:
echo("Commented-out code")
```
- `let` is used to create immutable variable bindings and `var` for mutable.
- when explicitly mentioning the type of a variable it is initialized to a default value
```Nim

var number: int # initialized to `0`
var text: string # initialized to `nil.nil`
```
- immutable variables (let bindings) can't be declared without initialization