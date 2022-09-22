# User and Message commands

So far only slash commands have been covered. There is two more types of application
commands: user context menu and message context menu commands. You can use these
by right clicking on a user or message respectively.

Both user and message commands are only supported as functions.

```python
@bot.include
@crescent.user_command
async def user_command(ctx: crescent.Context, user: hikari.User):
    ...


@bot.include
@crescent.message_command
async def message_command(ctx: crescent.Context, message: hikari.Message):
    ...
```
