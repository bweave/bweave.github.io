---
layout:     post
title:      "Ruby Enumerables: Each"
date:       2016-01-15
author:     Brian Weaver
summary:    Need to perform some action based on each item in an array, hash, or range? Each may be just what you're looking for.
category:   ruby
thumbnail:  list-ol
tags:
 - enumerables
 - each
---

Method Signatures

```ruby
each {|item| block } -> orig_array
```

```ruby
each_with_index {|item, index| block } -> orig_array
```

Ruby's [#each](https://ruby-doc.org/core-2.3.1/Array.html#method-i-each) and [#each_with_index](https://ruby-doc.org/core-2.3.1/Enumerable.html#method-i-each_with_index) methods call the given block once for each item in the array, and return the original array. For example, imagine you've got a Rails app for a school and you need to notify all students that classes are canceled.

```ruby
@students = Student.all
students.each {|student| Notification.send('Classes are canceled today!', student) }
```

What if you need to make use of the *index* of the iteration? Easy cheese!

```ruby
@students = Student.all
students.each_with_index do |student, i|
  Notification.send('Classes are canceled today!', student)
  logger.info("#{i}: #{student[:name]} has been notified.")
end
```
