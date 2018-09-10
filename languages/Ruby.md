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

