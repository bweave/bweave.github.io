---
layout: post
title: "Easily count occurences in an Array"
date: "2021-01-08 09:00:00 -0500"
tags: TIL Ruby
---

Counting occurrence in an Array is ezpz with `reduce` and `Hash.new(0)`.

```ruby
%w[dog cat dog dog].reduce(Hash.new(0)) do |pet, hash|
  hash[pet] += 1
end
 => {"dog"=>3, "cat"=>1}
```
