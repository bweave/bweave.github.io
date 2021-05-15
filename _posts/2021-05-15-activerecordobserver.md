---
layout: post
title: ActiveRecord::Observer
date: 2021-05-15 07:29:46 -0400
tags: ["TIL", "Rails"]
---

TIL `ActiveRecord::Observer` is a thing. You can use it to pull AR model callbacks out into dedicated classes. This seems useful if you've got a handful(s) of related callbacks and need a better way to organize them.

```ruby
class PostObserver < ActiveRecord::Observer
  def after_create(post)
    Subscribers.send_new_post_notification(post)
  end
end

class CommentObserver < ActiveRecord::Observer
  def after_create(comment)
    Subscribers.send_new_comment_notification(comment)
  end
end
```

As you might've guessed, the class to observe is inferred, so in the case where it can't be inferred, or multiple class should be watched, do this.

```ruby
class AuditObserver < ActiveRecord::Observer
  observe :account, :balance

  def after_update(record)
    AuditTrail.new(record, "UPDATED")
  end
end
```
