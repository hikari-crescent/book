# Your first command


Before you can create a command, you need to create a bot.

```python
import crescent
import hikari

bot = hikari.GatewayBot("YOUR_BOT_TOKEN")
client = crescent.Client(bot)

# `bot.run()` starts the bot.
# Any code after this line will not be run until the bot is closed. 
bot.run()

```

> ⚠️ Storing your token in your source code is a bad idea. In later sections,
> we will go over how to do this properly.


The first command we will make is the ping command.

```python

bot = hikari.GatewayBot("YOUR_BOT_TOKEN")
client = crescent.Client(bot)

# Commands can be defined after you create the client variable
# and before `bot.run()`

@client.include
@crescent.command(name="ping", description="Ping the bot.")
class PingCommand:
    async def callback(self, ctx: crescent.Context) -> None:
        await ctx.respond("Pong!")

bot.run()
```

> ⚠️ Commands must call `await ctx.respond()` within 3 seconds or call `await ctx.defer()` to
> get 15 minutes to respond.


So what's going on here? `@crescent.command` turns your class into a command object.
`@bot.include` adds the command to your bot. Many objects in Crescent can be added
to your bot with `@bot.include`, these are called Includables and we will go over
them in more detail later.

#### Type Hints

If you are new to Python, you may not have seen `ctx: crescent.Context` before. This
is called a type hint. It tells the reader what type `ctx` is, and your IDE can use
type hints to provide better autocomplete. Although they are not required, it is
recommended to use type hints whenever you can.

```python
#      The name of the argument   The return type
#                 \/                    \/
def my_function(argument: SomeType) -> None:
#                           /\
#                The type of the argument
#
# The argument name and argument type
# are separated with a colon.
```

## Adding Options

Options are added by adding class-attrs to the class.

```python
@client.include
@crescent.command(name="say")
class SayCommand:
# The name of the command option
#    \/
    word = crescent.option(str)
#                           /\
# The type of the command option

    async def callback(self, ctx: crescent.Context) -> None:
        # options are accessed attributes on the class
        await ctx.respond(self.word)
```

Crescent's option syntax is type safe. This means that commands will
seamlessly work with typecheckers like mypy and pyright.
(You don't need to worry about this if you are new to Python!)


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
