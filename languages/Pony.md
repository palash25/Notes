# Pony

- Pony is an object oriented, actor based language.
- Pony introduces the concept of reference capabilities to make concurrency safe. Reference capabilities are just type annotations that define the number of reference and read and write permissions for a type.
- Just like processes in Erlang/Elixir or goroutines in Go, Pony provides threads which are super lightweight, just 256 bytes as compared to Go's 4KB per goroutine.
- The highlight of Pony is no doubt its reference capabilities which are based on its type system.

## Classes

```pony
class Wombat
  let name: String
  var _hunger_level: U64
  var _thirst_level: U64 = 1

  new create(name': String) =>
    name = name'
    _hunger_level = 0

  new hungry(name': String, hunger': U64) =>
    name = name'
    _hunger_level = hunger'

  fun hunger(): U64 => _hunger_level

  fun ref set_hunger(to: U64 = 0): U64 => _hunger_level = to
```

`let` is used to define fields that are set just once, `var`s can be mutated.

### Reference capabilities of methods

The method `set_hunger` has a `ref` before it which indicates that the receiver (the object of Wombat) of the method is mutable since we need to set a field. In case of the method `hunger` which has no reference capability assigned to it, `box` is considered as the default.

The return value of `set_hunger` method is the old value of `_hunger_level`. Because in Pony even assignments are expressions rather than statements so they need to return a value. Unlike other languages Pony returns the old value which is known as a "destructive read". For e.g. swapping two number in Pony is a one liner `a = b = a` (b is set to the the value of a but the expression `b = a` returns the original value of b setting it to a)

## Primitives

```pony
// 2 "marker values"
primitive OpenedDoor 
primitive ClosedDoor

// An "enumeration" type
type DoorState is (OpenedDoor | ClosedDoor)

// A collection of functions
primitive BasicMath
  fun add(a: U64, b: U64): U64 =>
    a + b
  
  fun multiply(a: U64, b: U64): U64 =>
    a * b

actor Main
  new create(env: Env) =>
    let doorState : DoorState = ClosedDoor
    let isDoorOpen : Bool = match doorState
      | OpenedDoor => true 
      | ClosedDoor => false
    end
    env.out.print("Is door open? " + isDoorOpen.string())
    env.out.print("2 + 3 = " + BasicMath.add(2,3).string())
```

Primitives can have two special functions, _init and _final. _init is called before any actor starts. _final is called after all actors have terminated.

## Actors

They are just like classes but instead of methods/functions they have behaviours. Behaviours are pices of code that when called upon are executed asynchronously unlike functions.

```pony
actor Main
  new create(env: Env) =>
    call_me_later(env)
    env.out.print("This is printed first")

  be call_me_later(env: Env) =>
    env.out.print("This is printed last")
```

Since behaviours are async they return `None`. Actors are akin to Processes. When you call upon a behaviour on an actor you are actually passing a message to it.

Actors by themselves are synchronous, at a time we can call only one behaviour on an actor.

Actors can have finalisers, after executing a finaliser, actors can't receive messages.

## Subtyping
### Nominal Subtyping using Traits

```pony

trait Named
  fun name(): String => "Bob"
  
trait Bald
  fun hair(): Bool => false
 
class Bob is (Named & Bald)

class Larry
  fun name(): String => "Larry"

```

### Structural Subtyping using Interfaces

```pony
interface HasName
  fun name(): String
```

A class doesnt have to explicitly mention in this case that it belongs to the interface `HasName` both Bob and Harry *provide* HasName. Its the same as subtyping in Go and might lead to accidental subtyping.

## Structs

These are just classes used for FFI.

## Type Aliases

- [TODO](https://tutorial.ponylang.io/types/type-aliases.html)

