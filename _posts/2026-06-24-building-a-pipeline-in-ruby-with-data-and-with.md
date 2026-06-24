---
layout: post
title: "Building a data pipeline in Ruby"
date: 2026-06-24 09:00:00 -0400
tags: ["Ruby"]
---

Ruby 3.2 introduced [`Data`](https://docs.ruby-lang.org/en/3.2/Data.html), a class for simple immutable value objects. Unlike `Struct`, its members can't be changed after creation — and that constraint is exactly what you want when building a data pipeline.

Consider processing an invoice through a series of calculation steps:

```ruby
Invoice = Data.define(:line_items, :subtotal, :tax_rate, :tax, :total)

invoice = Invoice.new(
  line_items: [
    { name: "Widget", amount: 10_00 },
    { name: "Gadget", amount:  5_00 },
  ],
  subtotal: 0,
  tax_rate: 0.08,
  tax:      0,
  total:    0,
)
```

`with()` produces a new copy with specific members changed, leaving the original frozen:

```ruby
invoice.with(subtotal: 15_00)
# => #<data Invoice line_items=[...], subtotal=1500, tax_rate=0.08, tax=0, total=0>
```

Chain steps with `then`:

```ruby
processed = invoice
  .then { |inv| inv.with(subtotal: inv.line_items.sum { |i| i[:amount] }) }
  .then { |inv| inv.with(tax: (inv.subtotal * inv.tax_rate).round) }
  .then { |inv| inv.with(total: inv.subtotal + inv.tax) }

processed.total # => 1620
```

Each step receives an `Invoice`, reads what it needs, and returns a new `Invoice` with one field updated. Because nothing is mutated, steps can be extracted and tested independently:

```ruby
def calculate_subtotal(inv) = inv.with(subtotal: inv.line_items.sum { |i| i[:amount] })
def calculate_tax(inv)      = inv.with(tax: (inv.subtotal * inv.tax_rate).round)
def calculate_total(inv)    = inv.with(total: inv.subtotal + inv.tax)

processed = invoice
  .then { calculate_subtotal(_1) }
  .then { calculate_tax(_1) }
  .then { calculate_total(_1) }
```

The structure enforces a useful constraint: a step can only affect what it explicitly declares via `with()`. There's no accidental mutation of fields a step has no business touching.
