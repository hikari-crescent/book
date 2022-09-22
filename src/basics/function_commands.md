# Function Commands

Class commands can be cumbersome for small commands. Crescent provides function
commands for those cases.

```python
@bot.include
@crescent.command
async def ping(ctx: crescent.Context):
    await ctx.respond("Pong!")
```

It is recomended to use function commands when your command does not have any options.
