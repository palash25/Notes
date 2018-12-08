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
- `repeat-until` loop in Lua is the same as a `do-while`

- **Tail call optimization:** Lua is able to handle recursive function with large number of iterations provided that the recursive call is at the very end of the function
- Multiple return values are possible
- *Flow of control:* `while-do`, `for-do` and `if-then-elseif-then-else`

#### Tables

- Tables are like dictionaries.
- They can be written as key-value pairs between curly braces (also known as table constructor)
```lua
> book = {
>> title = "Grail Diary",
>> author = "Henry Jones",
>> pages = 100
>> }
```
- Access values using dot `book.title`
- New keys can be added and existing values can be modified using dot
- Keys that are calculated at runtime or keys with spaces and decimal points can be used with `[]` for e.g. `book[title]`
- Items can be removed by setting a key to `nil`
- Tables can also be used as arrays by ommitting the keys and just writing the values. The indicies start with 1 and values can be accessed using `[]`.
```lua
> medals = {
>> "gold",
>> "silver",
>> "bronze"
>> }
```
- Both the array and dictionary styles can be used together and be separated using a `;` (a best practice)

```lua
> ice_cream_scoops = {
>> "vanilla",
>> "chocolate";
>>
>> sprinkles = true
>> }
>>
=ice_cream_scoops[1]
vanilla
> =ice_cream_scoops.sprinkles
true
```
**Metatables (Overriding table's default behaviour)**

Every table in lua has a corresponding metatable which is set to `nil` by default.
```lua
>
=getmetatable(some_table)
nil
```
Metatables can be used to override the behaviour of tables but defining our own functions and assigning them to to a particular key (kind of like dunder functions in python) in the metatable

```lua
greek_numbers = {
 ena = "one",
 dyo = "two",
 tria = "three"
}

function table_to_string(t)
  local result = {}
  for k, v in pairs(t) do
    result[#result + 1] = k .. ": " .. v
  end
  return table.concat(result, "\n")
end

mt = {
  __tostring = table_to_string
  }

setmetatable(greek_numbers, mt)
```

Custom read/write logic can also be implemented
```lua
local mt = {
__index = strict_read,
__newindex = strict_write
}
treasure = {}
setmetatable(treasure, mt)

-- internal table to read the data of the actual table
local _private = {}

function strict_read(table, key)
  if _private[key] then
    return _private[key]
  else
    error("Invalid key: " .. key)
  end
end

function strict_write(table, key, value)
  if _private[key] then
    error("Duplicate key: " .. key)
  else
    _private[key] = value
  end
end
```

**User Defined OO Scheme**

Lua isn't OOP language but its prototypes provide a way to replicate OO patterns.
A table containing a set of common characteristics and methods can be used to create several objects with different states.

```lua
-- Prototype table
Villain = {
  health = 100,

  new = function(self, name)
    local obj = {
      name = name,
      health = self.health,
    }
    return obj
  end,

  take_hit = function(self)
    self.health = self.health - 10
    end
}

-- A new Villian object/table can be created using the prototype Villian
dietrich = Villain.new(Villain, "Dietrich")
```
But this `dietrich.take_hit(dietrich)` is not possible with the above prototype. To be able to override the lookup behaviour we can make use of metatables inside of our prototypes.

So now if the field is not found in the table `dietrich` it will look it up in our prototype `Villian` object. This can be done by making a few changes to the `new` function.

```lua
new = function(self, name)
  local obj = {
    name = name,
    health = self.health,
  }
  setmetatable(obj, self)
  self.__index = self
  return obj
end
```
So essentially all objects in Lua are copies of one object/prototype

**Inheritance**

It can be achieved by cloning an existing prototype

```lua
function SuperVillain.take_hit(self)
  -- Haha, armor!
  self.health = self.health - 5
end

-- Clone the newly created prototype.
toht = SuperVillain.new(SuperVillain, "Toht")
toht.take_hit(toht)
print(toht.health) --> 95
```

**Syntactic Sugar**

The self parameter can be ommitted when calling a method on a table

```lua
function Villain:new(name)
  -- ...same implementation as before...
end

function Villain:take_hit()
  -- ...same implementation as before...
end

SuperVillain = Villain:new()

function SuperVillain:take_hit()
  -- ...same implementation as before...
end

toht = SuperVillain:new("Toht")
toht:take_hit()
print(toht.health) --> 95
```

### Links
- [Lua Reference Manual](https://www.lua.org/manual/5.3/)
- [Learn X in Y minutes](https://learnxinyminutes.com/docs/lua/)
