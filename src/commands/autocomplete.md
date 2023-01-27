# Autocomplete

Autocomplete is a way for your command to suggest values for an option.
The `autocomplete=` kwarg can be used for `int`, `float`, and `str` types.

```python
async def autocomplete_response(
    ctx: crescent.AutocompleteContext, option: hikari.AutocompleteInteractionOption
) -> Sequence[hikari.CommandChoice]:
    return [
        hikari.CommandChoice(name="Some Option", value="1234")
    ]

@client.include
@crescent.command
class class_example:
    result = crescent.option(str, "Respond to the message", autocomplete=autocomplete_response)

    async def callback(self, ctx: crescent.Context) -> None:
        await ctx.respond(self.result, ephemeral=True)
```

Options can also be accessed inside the callback. The `ctx.options` dictionary contains snowflakes or
values for all the options a user has already filled out. The `ctx.fetch_values` function converts the
snowflakes in this dictionary to the correct type and returns it. If you bot object is `hikari.impl.CacheAware`
these values are fetched from the cache. Otherwise, they need to be fetched from a REST endpoint.

```python
async def fetch_autocomplete_options(
    ctx: crescent.AutocompleteContext, option: hikari.AutocompleteInteractionOption
) -> Sequence[hikari.CommandChoice]:
    # An option dict where discord objects are all snowflakes.
    options = ctx.options

    # Return options with snowflakes converted into the option types.
    options = await ctx.fetch_options()

    # Return no options.
    return []

bot.run()
```
