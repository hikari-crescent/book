# Your first command


Before you can create a command, you need to create a bot.

```python
import crescent

bot = crescent.Bot("YOUR_BOT_TOKEN")


# `bot.run()` starts the bot.
# Any code after this line will not be run until the bot is closed. 
bot.run()

```

> ⚠️ Storing your token in your source code is a bad idea. In later sections
> we will go over how to do this properly.


The first command we will make is the ping command.

```python

bot = crescent.Bot("YOUR_BOT_TOKEN")

# Commands can be defined after you create the bot variable
# and before `bot.run()`

@bot.include
@crescent.command(name="ping", description="Ping the bot.")
class PingCommand:
    async def callback(self, ctx: crescent.Context)
        await ctx.respond("Pong!")

bot.run()
```

So what's going on here? `@crescent.command` turns your class into a command object.
`@bot.include` adds the command to your bot. Many objects in Crescent can be added
to your bot with `@bot.include`, these are called Includables and we will go over
them in more detail later.

#### Type Hints

If you are new to Python you may not have seen `ctx: crescent.Context` before. This
is called a type hint. It tells the reader what type `ctx` is, and your IDE can use
type hints to provide better autocomplete. Although they are not required, it's
recommended to use type hints whenever you can.

```python
#      The name of the argument
#                 \/
def my_function(argument: SomeType)
#                           /\
#                The type of the argument
#
# The argument name and argument type
# are separated with a colon.
```

## Adding Options

Options are added by adding class-attrs to the class.

```python
@bot.include
@crescent.include(name="say")
class SayCommand:
# The name of the command option
#    \/
    word = crescent.option(str)
#                           /\
# The type of the command option

    async def callback(self, ctx: crescent.Context):
        # options are accessed attributes on the class
        await ctx.respond(self.word)
```

Crescent's option syntax is type safe.
(You don't need to worry about this if you are new to Python!)
