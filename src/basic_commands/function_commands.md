# Function Commands

Class commands can be cumbersome for small commands. Crescent provides function
commands for those cases.

```python
@client.include
@crescent.command
async def ping(ctx: crescent.Context):
    await ctx.respond("Pong!")
```

It is recommended to use function commands when your command does not have any options.
