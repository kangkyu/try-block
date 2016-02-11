# try Block

...it's really blocking.

I started with Chris Pine [book](https://pine.fm/LearnToProgram/), which starts with `puts` in **Chapter 2**. (`'Hello World'` shows in the **Introduction**, but that's not where I started) So that's why my first Ruby code was:

```rb
puts 1+2
# 3
# => nil
```
and so on.

```rb
v = 1+2
puts v
# 3
# => nil
```
variable, parameter, argument, object... and block.

```rb
{ puts 1+2 }
```
This is a block. It's "chunk of code" (When I google "chunk of code" and it shows Wikipedia Block on top... Maybe "chunky bacon" comes from it)

```rb
def say
  puts 1+2
end
say
# 3
# => nil
```
This is a method. You can call a method with method arguments, and it's nice because you can change those parameters from outside of method definition.

```rb
def say(a, b)
  puts a+b
end
say(1, 2)
# 3
# => nil
```
Likewise, it would look nicer if you can call it with a block.

```rb
def say
  yield
end
say { puts 1+2 }
# 3
# => nil
```
After a while, I've been seeing things like this. Here `say` doesn't do anything but "yield" for simplicity in this working code. (`yield` means "somewhere" - you really know what `yield` is, because it's right in the middle of every Rails app.) And in this case, `{}` is a block.

```rb
b = { puts 1+2 }
say b
# you wish
```
I wanted to do something like this. But it didn't work. 

First of all, it is something different. They say you omit parentheses when you can.

```rb
puts(1+2)
# 3
# => nil
say({ puts 1+2 })
# SyntaxError: (irb):1: syntax error, unexpected tINTEGER, expecting keyword_do or '{' or '('
# say({ puts 1+2 })
#             ^
# (irb):1: syntax error, unexpected '}', expecting end-of-input
# say({ puts 1+2 })
#                 ^
#   from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
say { puts 1+2 }
# 3
# => nil
```
And you don't "save" it into a variable, either.

```rb
v = 1+2
# => 3
b = { puts 1+2 }
# SyntaxError: (irb):1: syntax error, unexpected tSTRING_BEG, expecting keyword_do or '{' or '('
# b = { puts 1+2 }
#             ^
# (irb):1: syntax error, unexpected '}', expecting end-of-input
#   from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
```
Errors are only bad words. You don't read error messages, if you're beginning. Because it's humiliating!

I google. 

I found `-> {}` (stab).

```rb
b = -> { puts 1+2 }
# => #<Proc:0x007f951aa38bd8@(irb):1 (lambda)>
```
And it's working! No error!

It might have been saved. Used it.

```rb
b = -> { puts 1+2 }
say b
# ArgumentError: wrong number of arguments (given 1, expected 0)
#   from (irb):4:in `say'
#   from (irb):10
#   from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
```
Not working. So I need read more of it when google.

```rb
b.call
# 3
# => nil
```

# block or proc

I remembered `yield` is in other words `block.call` from Chris Pine book. He actually prefer `block.call` over `yield` in Chapter 14. (I remembered that. That's what I could do. Amazing because I forgot everything else on that Chapter)

```rb
def say &b
  b.call
end
say { puts 1+2 }
# 3
# => nil
b = -> { puts 1+2 }
say &b
# 3
# => nil
```
What is Ampersand(Et) doing here?

```rb
&b
# SyntaxError: (irb):17: syntax error, unexpected &
# 	from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
```
```rb
say(&b)
# 3
# => nil
say(&-> { puts 1+2 })
# 3
# => nil
say { puts 1+2 }
# 3
# => nil
-> { puts 1+2 }.call
# 3
# => nil
```
Okay.. so you get the idea. First do `->`, and then do `&`. It's "saved" and then "passed" somehow.

I wanted to say 3 three times.

```rb
def say
  yield
  yield
  yield
end
say { puts 1+2 }
# 3
# 3
# 3
# => nil
```

```rb
3.times { puts 1+2 }
b = -> { puts 1+2 }
3.times &b
# ArgumentError: wrong number of arguments (given 1, expected 0)
#   from (irb):13:in `block in irb_binding'
#   from (irb):14:in `times'
#   from (irb):14
#   from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
```

# Why?

which is an object? You know what **object** is? If you know, then you know everything in Ruby. "Everything is an object in Ruby" you always hear.

```rb
{ puts 1+2 } # not an object
-> { puts 1+2 } # object (and it's a "Proc" object)
b = -> { puts 1+2 }
&-> { puts 1+2 } # not an object
&b
```

```rb
# when doing something, it should be a block
say { puts 1+2 }

# when storing, it should be an object (it should be a proc)
b = -> { puts 1+2 }
b.call
```

# Again, why?


```rb
-> { puts 1+2 }
# => #<Proc:0x007fdd5aa2a6e0@(irb):15 (lambda)>
proc { puts 1+2 }
# => #<Proc:0x007fdd5aa19f70@(irb):16>
```
Stab is not all. "Go look at the documentation." (that's what you always hear, too. And I don't want to listen, because 9 out of 10 it's hard to read.) 

>
> ::new is the same as proc.
> 
> http://ruby-doc.org/core-2.3.0/Proc.html

You can use `Proc.new {}` instead of `proc {}` here.

```rb
Proc.new {}
# => #<Proc:0x007f951a97aea8@(irb):21>
proc {}
# => #<Proc:0x007f951a96a260@(irb):22>
```
These two do the same thing too.

```rb
lambda {} 
# => #<Proc:0x007f951a961cf0@(irb):23 (lambda)>
-> {}
# => #<Proc:0x007f951a9599d8@(irb):24 (lambda)>
```
And 

```rb
b = proc { puts 1+2 }
3.times &b
# 3
# 3
# 3
# => 3
```
Is working!

But why? I like the "stabby lambda"!

`Integer#times` has this description.
>
> Iterates the given block int times, passing in values from zero to int - 1.
>
> ```rb
  5.times do |i|
    print i, " "
  end
  #=> 0 1 2 3 4
``` 
> http://ruby-doc.org/core-2.3.0/Integer.html#method-i-times

```rb
b = -> { |i| puts 1+2 }
3.times &b
# 3
# 3
# 3
# => 3
```

Again, why?

```rb
b = -> { puts 1+2 }
3.times &b
# ArgumentError: wrong number of arguments (given 1, expected 0)
```
Now you read errors. It says wrong number of arguments.

> Returns true for a Proc object for which argument handling is rigid. Such procs are typically generated by lambda.
> 
> http://ruby-doc.org/core-2.3.0/Proc.html#method-i-lambda-3F

Therefore, block parameters are necessary for a lambda. Not for a proc.

Block parameters, block arguments.. for another day. I think it would be enough even with no-parameter blocks only.

# The end
