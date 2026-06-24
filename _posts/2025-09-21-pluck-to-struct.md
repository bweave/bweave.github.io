---
layout: post
title: Pluck to Struct
date: 2025-09-21 08:00:00 -0800
tags: ["Ruby", "Rails", "ActiveRecord"]
---

## The Problem

On a recent project, rendering a large XML feed from full ActiveRecord objects and associations was taking upward of 10 seconds. The feed only needed a handful of columns, but we were loading entire models, their associations, and triggering N+1 queries along the way. Combining ActiveRecord's `.pluck` with Struct dropped rendering time to under 300ms.

## The Solution

Use `pluck` to fetch only the columns you need in a single query, then map each row directly to a Ruby `Struct`. You get structured, readable data objects without any ActiveRecord overhead.

```ruby
class Author < ApplicationRecord
  # schema: id, first_name, last_name
  has_many :posts
end

class Post < ApplicationRecord
  # schema: id, title, body, published, published_at, author_id
  belongs_to :author
end

# In a real app, define this inside the class that uses it or in app/value_objects/
PostSummary = Struct.new(:id, :title, :published_at, :author_first_name, :author_last_name) do
  def author_name = "#{author_first_name} #{author_last_name}"
end

posts = Post.joins(:author)
  .where(published: true)
  .order(published_at: :desc)
  .limit(25)
  .pluck("posts.id", "posts.title", "posts.published_at", "authors.first_name", "authors.last_name")
  .map { |row| PostSummary.new(*row) }

post = posts.first
post.title       # => "Hello, World!"
post.author_name # => "Brian Weaver"
```

`pluck` returns raw values — no AR object overhead. We map each row into a `PostSummary` Struct, which gives dot-syntax access and a place to put computed methods like `author_name`.

## The Benefits

**Performance**: One query, only the columns you need. No N+1, no unused associations loaded into memory.

**Memory efficiency**: Struct instances are significantly lighter than AR models.

**Clarity**: The Struct definition is an explicit contract — it lists exactly what the consumer needs.

**Testability**: Structs are easy to build in tests without factories or fixtures: `PostSummary.new(1, "Hello", Time.now, "Brian", "Weaver")`.

This pattern works especially well for read-only views like feeds, reports, and API serializers.
