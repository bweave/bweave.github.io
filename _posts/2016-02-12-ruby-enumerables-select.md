---
layout:     post
title:      "Ruby Enumerables: Select"
date:       2016-02-12
author:     Brian Weaver
summary:    Need to filter down a group of items to a select few? Select may be just what you're looking for.
category:   ruby
thumbnail:  filter
tags:
 - enumerables
 - select
---

Method Signature

```ruby
select { |obj| block } -> array
```

Ruby's [#select](https://ruby-doc.org/core-2.3.1/Enumerable.html#method-i-select) method calls the given block once for each item in the collection (array or hash), and returns all items where the block returns true. For example:

```ruby
marvel_characters = [
  { name: 'Captain America', team: 'Avengers' },
  { name: 'Iron Man',        team: 'Avengers' },
  { name: 'Spiderman',       team: 'solo' },
  { name: 'Venom',           team: 'villains' },
  { name: 'Wolverine',       team: 'X-Men' },
  { name: 'Apocalypse',      team: 'villains' },
  { name: 'Star Lord',       team: 'Guardian of the Galaxy' }
]

villains = marvel_characters.select{|character| character[:team] == 'villains'}

# [
#   { name: 'Venom', affiliation: 'villains' },
#   { name: 'Apocalypse', affiliation: 'villains' }
# ]
```
