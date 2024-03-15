# Ruby Pro Tips

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
