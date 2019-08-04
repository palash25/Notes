## Learning Ruby from 7 languages in 7 weeks

### Key features
- Interpreted
- OOP
- Strongly Typed
- Duck Typing
- Conditionals can be written in both one line or in blocks. Ex-

```ruby
if x == 4
   puts 'This appears to be true.'
end

puts 'This appears to be true.' if x == 4
```

- Instead of `not` or `!` `until` and `unless` can be used for negation and looks much cleaner than the former.

- Arrays (like python list) and Hash (like python dicts) are 2 major data structures in Ruby

- Hashes have a `label-object` pair. Ex

```ruby
 numbers = {1 => 'one', 2 => 'two'}

 stuff = {:array => [1, 2, 3], :string => 'Hi, mom!'}
```

> The labels with colon are called symbols and are unique objects so the symbols with the same name are the same physical object
Symbols are lightweight strings used when we don't want the strings to be printed to the screen

- Capitalized variables in Ruby are constants e.g `EmpireStateBuilding = "350 5th Avenue, NYC, NY"`

- There are no named parameters in Ruby but this concept can be emulated using a hash

```ruby
def truth(option={})
	if option[:profession] == :lawyer
		puts 'could be false'
	else
		puts 'true'
	end
end

truth

truth :profession => :lawyer
```

- A code block is a function without a name `3.times {puts 'hiya there, kiddo'}` will execute the block/function 3 times.

- For 1 line blocks braces are used and for code blocks exceeding that limit we use `do/end`

- Passing executable code around in Ruby
```ruby
>> def call_block(&block)
>> block.call
>> end
=> nil
>> def pass_block(&block)
>> call_block(&block)
>> end
=> nil
>> pass_block {puts 'Hello, block'}
Hello, block
```

- Ruby equivalent for pythons *args and *kwargs are called splat(*) and double splat operator(**). Example:
```ruby
def test_method
  yield({hash_one: "argument one", hash_two: "argument two", hash_three: "argument three"})
end

test_method do |**double_splat_parameter|
  puts double_splat_parameter
end

```
**Classes**
- In ruby classes can only inherit from one parent known as a "superclass". Example
```
>> 4.class
=> Fixnum
>> 4.class.superclass
=> Integer
>> 4.class.superclass.superclass
=> Numeric
>> 4.class.superclass.superclass.superclass
=> Object
>> 4.class.superclass.superclass.superclass.superclass
=> nil
```

- Anything used after a dot is a method. Methods can be chanined together. For e.g. `"  some String   ".trim.capitalize`

- *Class Methods:* Unlike usual methods class methods in Ruby are attached to variables/constants using (`::`). E.g. `Network::Connect`

- *Global variables:* Represented using `$variable_name`
- *Instance variables:* Using @ symbol `@var`. Used to define attributes of an object
- *Class Variables*: Represented by double ats. E.g. `@@class_var`


- *Concatanation:* The `<<` method is used for this. 


 Classes start
with capital letters and typically use CamelCase to denote capitalization. You must prepend instance variables (one value per object) with
@ and class variables (one value per class) with @@. Instance variables
and method names begin with lowercase letters in the underscore_style.
Constants are in ALL_CAPS. Functions and
methods that test typically use a question mark (if test?).

The attr keyword defines an instance variable and a method of the same name to access it

Modules are ruby's equivalent to Java's interfaces and provide a way to replace multiple inheritance.

Module: a bunch of functions and constants that when included into classes becomes a part of it.

> This is called implementing a Mixin

Ex:

```ruby

module TestModule
	def func_1
		...
	end

	def func_2
		...
	end
end

class Something
include TestModule
	def initialize()
		...
	end
end

Something.new(...).func_1
```

Functions implemented in the class can also be used in a module.


