---
layout: post
title: "ActiveRecord upsert_all"
date: "2020-11-20 09:00:00 -0500"
tags: TIL Ruby Rails ActiveRecord
---

ActiveRecord has an [upsert_all](https://api.rubyonrails.org/classes/ActiveRecord/Persistence/ClassMethods.html#method-i-upsert_all). It "... updates or inserts (upserts) multiple records into the database in a single SQL INSERT statement" without instantiating ActiveRecord objects.

So the next time you need to update multiple records, like in my case so set the `position` attribute of multiple drag-n-drop-ed records, you can do something like this and only make two database queries!

```ruby
ids_and_positions = {1: 2, 2: 3, 3: 1}
records = MyRecord.where(id: ids_and_positions.keys)
records_attributes = records.map do |record| # first query, thanks AR lazy loading!
  record.attributes.merge(position: ids_and_position[record.id])
end
MyRecord.upsert_all(records_attributes) # second query
```

It's worth being aware that this is a "sharp knife" tool and bypasses validations.

Also, pay special attention to the nuances of the `returning` and `unique_by` args in the docs.
