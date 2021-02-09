---
layout: post
title: Keeping Rails logs slim and trim
date: 2021-02-09 10:53 -0500
tags: TIL Rails
---

Add this to `development.rb` and `test.rb` in `config/environments/` to clean up your development and test logs daily.

```ruby
config.logger = ActiveSupport::Logger.new(config.paths['log'].first, "daily")
```

Check out the [docs](https://ruby-doc.org/stdlib-2.7.2/libdoc/logger/rdoc/Logger.html#method-c-new) for more info.
