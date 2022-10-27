# Cooldowns

This module allows you to rate limit users with a sliding window rate limit.

The `crescent.ext.cooldowns` module provides a hook.

- `capacity` is the amount of times the command can be used in a timeframe.
- `period` is the length of this timeframe.

```python
import crescent
from crescent.ext import cooldowns

@bot.include
# This command be used 3 times in 20 seconds.
@crescent.hook(cooldowns.cooldown(capacity=3, period=20))
@crescent.command
async def cooldowned(ctx: crescent.Context):
    print("Doing expensive operation...")
    await ctx.respond("Hello!")
```


## Rate Limited Hook
Callbacks can be set to run when a user is ratelimited.

```python
async def on_rate_limited(ctx: crescent.Context, time_remaining: float) -> None:
    await ctx.respond(f"You are ratelimited for {time_remaining}s.")

@bot.include
@crescent.hook(cooldowns.cooldown(1, 100, callback=on_rate_limited))
@crescent.command
async def cooldowned(ctx: crescent.Context) -> None:
    print("Doing expensive operation...")
    await ctx.respond("Hello!")
```

## Custom Bucket

The default bucket uses user IDs to seperate users.

This is how the default bucket is implemented:
```python
import typing
import crescent

def default_bucket(ctx: crescent.Context) -> typing.Any:
    return ctx.user.id
```

This is a bucket that rate limits users based on ID and guild ID:
```python
import crescent
import typing

def custom_bucket(ctx: crescent.Context) -> typing.Any:
    return f"{ctx.guild_id}{ctx.user.id}"
```

To use a custom bucket, pass it into the `bucket` kwarg.

```python
@bot.include
@crescent.hook(cooldowns.cooldown(3, 20, bucket=custom_bucket))
@crescent.command
async def cooldowned(ctx: crescent.Context):
    print("Doing expensive operation...")
    await ctx.respond("Hello!")
```
