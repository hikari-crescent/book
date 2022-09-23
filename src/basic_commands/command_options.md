# Command Options

As seen in the previous section, command options can be created with the `crescent.option` function.

This is what a command with an option called `name` looks like in the Discord client..

![](https://i.imgur.com/NXAJnZv.png)


Options can also have a custom description and name. If no description is provided,
the description will default to "No Description". This example shows an option 
amed "option" with the description "your custom description". The secondoption, `option2`,
has the name "custom-name" and description "also your custom description".

```python
@bot.include
@crescent.command
class MyCommand:
    option = crescent.option(str, "your custom description")
    option2 = crescent.option(str, name="custom-name", description="also your custom description")

    async def callback(self, ctx: crescent.Context) -> None:
        ...

    # The `...` is a placeholder that means that your code
    # should go there instead.
```

## Option Types
Crescent provides these option types. You can find more information on option types [here](https://discord.com/developers/docs/interactions/application-commands#application-command-object-application-command-option-type) (You can ignore `SUBCOMMAND` and `SUBCOMMAND_GROUP` for now.)
This might look a bit daunting, but we will go into detail on what each option type is in this section.

| Type | Option Type |
|---|---|
| `str` | Text |
| `int` | Integer |
| `bool` | Boolean |
| `float` | Number |
| `hikari.User` | User |
| `hikari.Role` | Role |
| `crescent.Mentionable` | Role or User |
| Any Hikari channel type. | Channel. The options will be the channel type and its subclasses. |
| `List[Channel Types]` | Channel. ^ |
| `hikari.Attachment` | Attachment |

## Making options optional
Options will be optional if a default value is provided. This example
shows an option with the default value `None`.

```python
@bot.include
@crescent.command(name="command")
class MyCommand:
    optional_option = crescent.option(str, default=None)

    async def callback(self, ctx: crescent.Context) -> None:
        ...
```


## More Information On Types
Strings, Ints, Floats, and Booleans all use python's built in types.

> If you are comfortable reading function overloads you can look at
> [the source code](https://github.com/magpie-dev/hikari-crescent/blob/main/crescent/commands/options.py#L140).

```python
@bot.include
@crescent.command(name="command")
class MyCommand:
    word = crescent.option(str)
    integer = crescent.option(int)
    number = crescent.option(float)
    boolean = crescent.option(bool)

    async def callback(self, ctx: crescent.Context) -> None:
        # You can now do something with the options.
        await ctx.respond(
            f"{self.word}\n{self.integer}\n{self.number}\n{self.boolean}"
        )
```

These types use a hikari object.
```python
import hikari

@bot.include
@crescent.command(name="command")
class MyCommand:
    user = crescent.option(hikari.User)
    role = crescent.option(hikari.Role)
    attachment = crescent.option(hikari.Attachment)

    # The channel type will be restricted depending on what
    # channel object you choose. In this example only channels
    # that users can type in can be chosen.
    channel = crescent.option(hikari.TextableChannel)

    # This option can only be voice channels.
    voice_channel = crescent.option(hikari.GuildVoiceChannel)


    async def callback(self, ctx: crescent.Context) -> None:
        ...
```

The final option type is mentionable, which allows a user to choose a user or role.

```python
import hikari

@bot.include
@crescent.command(name="command")
class MyCommand:
    mentionable = crescent.option(crescent.Mentionable)

    async def callback(self, ctx: crescent.Context) -> None:
        if self.mentionable.user:
            # This is a user. `mentionable.role` will be `None`.
            await ctx.respond("You picked a user!")
        if self.mentionable.role:
            # This is a role. `mentionable.user` will be `None`.
            await ctx.respond("You picked a role!")
```
