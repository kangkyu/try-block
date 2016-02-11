# Wait a minute!

```
(1..3).collect &:to_s
# => ["1", "2", "3"]
```

That was not the end.. while I was reading that lambda [page](http://ruby-doc.org/core-2.3.0/Proc.html#method-i-lambda-3F)


```rb
def n(&b) b.lambda? end
# => :n
def m() end
# => :m
n(&method(:m))
# => true
n(&method(:m).to_proc)
# => true
```

> Returns a Proc object which respond to the given method by sym.
>
> ```rb
(1..3).collect(&:to_s)  #=> ["1", "2", "3"]
```
> http://ruby-doc.org/core-2.3.0/Symbol.html#method-i-to_proc
>

```
ri to_proc
```
This is when `ri` really shines. You found `Symbol#to_proc` and `Method#to_proc`. Finally, what I tried was

```rb
method(:to_s)
# => #<Method: main.to_s>
method(:to_s).to_proc
=> #<Proc:0x007fb03c809898 (lambda)>
(1..3).collect &method(:to_s).to_proc
# ArgumentError: wrong number of arguments (given 1, expected 0)
#   from (irb):38:in `to_s'
#   from (irb):38:in `each'
#   from (irb):38:in `collect'
#   from (irb):38
#   from /Users/klee/.rbenv/versions/2.3.0/bin/irb:11:in `<main>'
```
I think I know why it's not working. It's a lambda, and collect method takes a block with a **block parameter**, which matters to a lambda.

```rb
:to_s.to_proc
=> #<Proc:0x007f9ddb9f2b28(&:to_s)>
```

So it's not a lambda. you can do

```rb
(1..3).collect &:to_s.to_proc
=> ["1", "2", "3"]
```

It looks like you don't need `.to_proc` at the end in this case.

```rb
(1..3).collect &method(:to_s)
# ArgumentError: wrong number of arguments (given 1, expected 0)
(1..3).collect &:to_s
# => ["1", "2", "3"]
```
