---
layout: post
title: A registry pattern in Ruby
date: 2024-01-03 13:21:08 -0800
tags: ["Ruby"]
---

The registry pattern gives you a central directory where objects self-register by key at load time. Instead of maintaining a hardcoded list of implementations, each class announces itself when its file is loaded. The registry grows as you add files; nothing else changes.

Here's a `Formatter` registry with two implementations in separate files:

```ruby
# formatter.rb
class Formatter
  @registry = {}

  class << self
    attr_reader :registry

    def register(key, klass)
      registry[key] = klass
    end

    def for(key)
      registry.fetch(key) { raise ArgumentError, "Unknown formatter: #{key.inspect}" }
    end
  end
end

# csv_formatter.rb
class CsvFormatter
  Formatter.register(:csv, self)

  def format(rows)
    rows.map { |row| row.join(",") }.join("\n")
  end
end

# json_formatter.rb
class JsonFormatter
  Formatter.register(:json, self)

  def format(rows)
    rows.to_json
  end
end
```

`Formatter.register(:csv, self)` runs at class definition time — not instantiation. In a Rails app, as each file is autoloaded, the formatter announces itself. By the time your app boots, the registry is fully populated.

```ruby
Formatter.for(:csv).new.format([["name", "age"], ["Alice", 30]])
# => "name,age\nAlice,30"

Formatter.for(:json).new.format([["name", "age"], ["Alice", 30]])
# => "[[\"name\",\"age\"],[\"Alice\",30]]"

Formatter.registry
# => {csv: CsvFormatter, json: JsonFormatter}
```

Adding an `XlsxFormatter` means creating one new file that calls `Formatter.register(:xlsx, self)`. `Formatter` itself never changes.
