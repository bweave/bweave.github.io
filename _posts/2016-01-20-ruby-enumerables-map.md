---
layout:     post
title:      "Ruby Enumerables: Map"
date:       2016-01-20
author:     Brian Weaver
summary:    Need a new array pieced together from an array you've already got? Map may be just what you're looking for.
category:   ruby
thumbnail:  map
tags:
 - enumerables
 - map
---

Method Signature

```ruby
map {|obj| block } -> new_array
```

Ruby's [#map](https://ruby-doc.org/core-2.3.1/Enumerable.html#method-i-map) method creates a new array from whatever is returned by the block each time it runs. It's part of the Enumberable module, so it's available out of the box on arrays and hashes. Here's an example of how to use it on an array:

```ruby
users = [
  { name: 'John',    age: 35 },
  { name: 'Susie',   age: 15 },
  { name: 'Dilbert', age: 45 },
  { name: 'Craig',   age: 25 },
  { name: 'Anne',    age: 65 }
]

usernames = users.map {|user| user[:name]}

# [ 'John', 'Susie', 'Dilbert', 'Craig', 'Anne' ]
```

With hashes, you get two parameters passed to the block:

```ruby
pirates = {
    'Henry Morgan' => 'male',
    'Blackbeard'   => 'male',
    'Anne Bonnie'  => 'female',
    'Madame Cheng' => 'female'
}

names = pirates.map {|name, gender| name}

# ['Henry Morgan', 'Blackbeard', 'Anne Bonnie', 'Madame Cheng']

```
