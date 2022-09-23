# A Look Into Context

The `crescent.Context` object allows you to respond to interactions.
In this chapter we will look at the methods it has and using a custom context object.

### Attributes

These are the only methods you will need to use on the `Class` class. They are all async.

- `Context.respond` - Respond to an interaction.
- `Context.defer` - Defer a message, giving you 15 minutes to respond instead of 3 seconds.
- `Context.edit` - Edit a response to an interaction.

This is a list of useful properties on the `Context` class.

- `interaction` The interaction object.
- `app` - The application instance. (Your bot object)
- `channel_id`- The channel ID of the channel that the interaction was used in.
- `channel`- The channel that the interaction was used in, if the channel is cached.
- `guild_id` - The guild ID of the guild that this interaction was used in, if used in a guild.
- `guild` - The guild that this interaction was used in, if the guild is cached.
- `user`- The user who triggered this command interaction.
- `member` - The member object for the user that triggered this interaction, if used in a guild.


### Custom Context
You may want to use a custom context class for a variety of reasons.
This example shows how to use a custom context class for better typing
information.

```python
class MyBotClass(crescent.Bot):
    ...

class CustomContext(crescent.Context):
    app: MyBotClass


async def my_command(ctx: CustomContext):
    reveal_type(ctx.app) # `MyBotClass`
```

You can also inherit from `crescent.BaseContext` if you dont want any of
`crescent.Context`'s attributes in your class.

> ⚠️ This is not recommended if you are new to the library.

```python
class CustomContext(crescent.Context):
    ...

async def my_command(ctx: CustomContext):
    reveal_type(ctx.app)
```
