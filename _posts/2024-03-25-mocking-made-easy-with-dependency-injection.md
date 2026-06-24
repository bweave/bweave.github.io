---
layout: post
title: Mocking made easy with dependency injection
date: 2024-03-25 08:53:08 -0700
tags: ["Ruby"]
---

When a class hardcodes its collaborators like this:

```ruby
class SlackNotifier
  def notify(message)
    # posts to Slack
  end
end

class OrderProcessor
  def process(order)
    total = order[:subtotal] * (1 - order[:discount])
    SlackNotifier.new.notify("Order #{order[:id]} processed: $#{total}")
    total
  end
end
```


...testing it requires reaching for a mocking library to intercept those calls. If you inject the collaborator through the initializer instead, then a plain Struct is all you need for a fake.

```ruby
class SlackNotifier
  def notify(message)
    # posts to Slack
  end
end

class OrderProcessor
  def initialize(notifier_klass: SlackNotifier)
    @notifier_klass = notifier_klass
  end

  def process(order)
    total = order[:subtotal] * (1 - order[:discount])
    @notifier_klass.new.notify("Order #{order[:id]} processed: $#{total}")
    total
  end
end
```

In tests, pass a fake that satisfies the same interface:

```ruby
class OrderProcessorTest < ActiveSupport::TestCase
  FakeNotifier = Struct.new do
    def notify(_message) = nil
  end

  test "applies discount to order total" do
    order = { id: 1, subtotal: 100.0, discount: 0.10 }
    assert_equal 90.0, OrderProcessor.new(notifier: FakeNotifier).process(order)
  end
end
```

`FakeNotifier` is a memberless Struct — `FakeNotifier.new` takes no arguments, matching the interface `OrderProcessor` expects. The Slack call is silenced, and the test focuses on what `OrderProcessor` actually does: calculate the discounted total. No mocking library needed.

The default initializer argument (`notifier_klass = SlackNotifier`) means production code is untouched. Pass a fake only when you need to.
