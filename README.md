# Ruby on Rails Pro Tips

### Use `Time#getutc` instead of `Time#utc`
```rb
# ❌
now = Time.now

now        # 2024-03-15 09:42:43.57382 +0100
now.utc    # 2024-03-15 08:42:43.57382 UTC
now        # 2024-03-15 08:42:43.57382 UTC

# ✅

now = Time.now

now         # 2024-03-15 09:42:43.57382 +0100
now.getutc  # 2024-03-15 08:42:43.57382 UTC
now         # 2024-03-15 09:42:43.57382 +0100
```

###
```rb
class User < ApplicationRecord
  has_many :orders
  has_many :reviews

  # ❌
  scope :recent_orders, -> {
    joins(:orders).where(orders: { created_at: 30.days.ago.. })
  }

  scope :top_rated, -> {
    joins(:reviews).where(reviews: { rating: 4.. })
  }
  # This fails! Returns 4 instead of 1
  # Because: 2 orders x 2 reviews = 4 records
  User.bad_recent_orders.bad_top_rated.count # => 4

  # ✅
  scope :recent_orders, -> {
    where(id: Order.recent_orders.select(:user_id))
  }
  scope :top_rated, -> {
    where(id: Review.top_rated.select(:user_id))
  }
  # This works! Will not miscount!
  # PLUS scopes are DRY-ly defined where they belong!
  # PLUS multiple models can have the same scope behavior!
  User.good_recent_orders.good_top_rated.count # => 1
end
```

### Use `normalizes` and `.presence` to convert empty Strings to nil for consistency

```rb
class Account < ApplicationRecord
  normalizes :billing_email, with: -> { it.presence }
end
```
Check more [here](https://x.com/excid3/status/1876748745771028796)
