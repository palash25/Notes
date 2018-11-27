## From the 7 in 7 series

- Prototype based langauges
> In prototype based languages the class and the instances are not separate entities. Once an object/instance is defined it doesn't serve as a blueprint for other objects but several copies of the same object are made that can then be mutated.

- Supports procedural, object-oriented, event-driven programming

### Basic features
- Concatanation `'abc' .. 'xyz'`
- Variadic functions are possible
```lua
function something(a, ...)
  otherArgs = {..}
  return otherArgs[1]
```
- If lesser args are passed to a funtion call the the parameters that don't receive an argument get initialized to `nil`
- More arguments are passed then they are ignored
- Functions are first class citizen and can be stored in variables and passsed around as arguments
- `#'fdsf'` gives the length of the string

- **Tail call optimization:** Lua is able to handle recursive function with large number of iterations provided that the recursive call is at the very end of the function
- Multiple return values are possible
- *Flow of control:* `while-do`, `for-do` and `if-then-elseif-then-else`
