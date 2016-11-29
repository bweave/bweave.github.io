---
layout:     post
title:      "Refactoring to Collections with Ruby"
date:       2016-06-15
author:     Brian Weaver
summary:    Adam Wathan's book "Refactoring to Collections" is a fantastic example of how functional programming can be leveraged in OOD. Let's look at how we can take some of his PHP examples and implement them in Ruby.
category:   ruby
thumbnail:  fork
tags:
 - enumerables
 - reduce
 - map
 - each
 - select
 - max_by
 - sort_by
---

Adam Wathan's book ["Refactoring to Collections"](https://adamwathan.me/refactoring-to-collections/) is a fantastic example of how functional programming can be leveraged in OOP. Let's look at how we can take some of his PHP examples and implement them in Ruby.

## 1. Extract Marketing Dept. Emails

Given we have an array of employess from different departments...

```ruby
employees = [
  { name: 'John', department: 'Sales',       email: 'john3@example.com' },
  { name: 'Jane', department: 'Marketing',   email: 'jane8@example.com' },
  { name: 'Dave', department: 'Marketing',   email: 'dave1@example.com' },
  { name: 'Dana', department: 'Engineering', email: 'dana8@example.com' },
  { name: 'Beth', department: 'Marketing',   email: 'beth4@example.com' },
  { name: 'Kyle', department: 'Engineering', email: 'kyle8@example.com' },
]
```

How would we write a collection pipeline to extract just the Marketing Dept. employee emails?

```ruby
marketing_emails = employees.select {|employee| employee[:department] == 'Marketing' }.map {|employee| employee[:email] }

# [
#   "jane8@example.com",
#   "dave1@example.com",
#   "beth4@example.com"
# ]
```

## 2. Employee Count per Dept.

Given we have an array of employess from different departments...

```ruby
employees = [
  { name: 'John',  department: 'Sales',       email: 'john3@example.com' },
  { name: 'Jane',  department: 'Marketing',   email: 'jane8@example.com' },
  { name: 'Dave',  department: 'Marketing',   email: 'dave1@example.com' },
  { name: 'Dana',  department: 'Engineering', email: 'dana8@example.com' },
  { name: 'Beth',  department: 'Marketing',   email: 'beth4@example.com' },
  { name: 'Kyle',  department: 'Engineering', email: 'kyle8@example.com' },
  { name: 'Steve', department: 'Sales',       email: 'steve7@example.com' },
  { name: 'Liz',   department: 'Engineering', email: 'liz6@example.com' },
  { name: 'Joe',   department: 'Marketing',   email: 'joe5@example.com' },
]
```

How would we write a collection pipeline that returns a hash where the key is the department name and the value is the number of employees?

```ruby
dept_counts = employees.reduce(Hash.new(0)) do |results, employee|
  results[ employee[:department] ] += 1
  results
end

# {
#   "Sales" => 2,
#   "Marketing" => 4,
#   "Engineering" => 3
# }
```

## 3. Employee with the Most Valuable Sale

Given we have an array of employees and their sales...

```ruby
employees = [
  {
    name: 'John',
    email: 'john3@example.com',
    sales: [
      { customer: 'The Blue Rabbit Company', order_total: 7444 },
      { customer: 'Black Melon',             order_total: 1445 },
      { customer: 'Foggy Toaster',           order_total: 700 },
    ],
  },
  {
    name: 'Jane',
    email: 'jane8@example.com',
    sales: [
      { customer: 'The Grey Apple Company',  order_total: 203 },
      { customer: 'Yellow Cake',             order_total: 8730 },
      { customer: 'The Piping Bull Company', order_total: 3337 },
      { customer: 'The Cloudy Dog Company',  order_total: 5310 },
    ],
  },
  {
    name: 'Dave',
    email: 'dave1@example.com',
    sales: [
      { customer: 'The Acute Toaster Company', order_total: 1091 },
      { customer: 'Green Mobile',              order_total: 2370 },
    ],
  },
]
```

How would we write a collection pipeline to find the employee who made the mose valuable sale?

```ruby
mvp_employee = employees.max_by do |employee|
  highest_sale = employee[:sales].max_by {|s| s[:order_total] }
  highest_sale[:order_total]
end

# {
#   :name => "Jane",
#   ...
# }
```

Or as a one-liner:

```ruby
mvp_employee = employees.max_by {|emp| emp[:sales].max_by {|s| s[:order_total] }[:order_total] }

# {
#   :name => "Jane",
#   ...
# }
```
