---
layout: post
title: "Count takes a block"
date: "2020-12-04 08:00:00 -0500"
tags: TIL Ruby Crystal
---

[Ruby](https://ruby-doc.org/core-2.5.1/Enumerable.html#method-i-count) and [Crystal's](https://crystal-lang.org/api/0.35.1/Enumerable.html#count(&)-instance-method) `#count` can both take a `block` and return the number of elements in the collection where the block returns true.

```ruby
[1, 2, 3].count { |num| num > 2 }
#=> 1
```

Count the differences between two strings:

```ruby
string_a = "ABC"
string_b = "CDE"
string_a.chars.zip(string_b.chars).count { |(a, b)| a != b }
#=> 4
```
