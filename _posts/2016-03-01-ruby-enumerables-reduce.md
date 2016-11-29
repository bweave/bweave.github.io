---
layout:     post
title:      "Ruby Enumerables: Reduce"
date:       2016-03-01
author:     Brian Weaver
summary:    Need to ...? Reduce may be just what you're looking for.
category:   ruby
thumbnail:  level-down
tags:
 - enumerables
 - reduce
 - inject
---

Method Signatures

```ruby
reduce(sym) → obj

reduce(initial, sym) → obj

reduce { |memo, obj| block } → obj

reduce(initial) { |memo, obj| block } → obj
```

Ruby's [#reduce](https://ruby-doc.org/core-2.3.1/Enumerable.html#method-i-reduce) method combines all items in a collection by applying a symbol that names a method or operator, like `:+`, or a block. Because of its flexibility, the method signature has several variants. 

Using a symbol with *no* initial value

```ruby
(1..3).reduce(:+)

#=> 6
```

Using a symbol *with* an initial value

```ruby
initial_value = 1
(1..3).reduce(initial_value, :+)

#=> 7
```

Using a block with *no* initial value

```ruby
(1..3).reduce {|sum, item| sum + item }

#=> 6
```

Using a block *with* an initial value

```ruby
initial_value = 1
(1..3).reduce(initial_value) {|sum, item| sum + item }

#=> 7
```
