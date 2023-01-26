# Model

Passing information between different files is hard. That's why crescent provides the `model` attribute. A `model` is any object you want that will be dependency-injected into your plugins.

```python
import dataclasses
import hikari
import crescent


# The example model is a dataclass. This class can be whatever you want.
@dataclasses.dataclass
class Model:
    value = 5

bot = hikari.GatewayBot("TOKEN")
client = crescent.Client(bot, Model())
```

You should also update your plugin type alias to use the model you created.

```python
Plugin = crescent.Plugin[hikari.GatewayBot, Model]()
```

After the plugin is loaded, you can access your model with the `model` property.

```python
# The plugin option created in the previous code block.
plugin = Plugin()

@plugin.load_hook
def on_load():
    print(plugin.model)
```

## Tips and Tricks
### Objects That Need to be Created in an Async Function

Its common to have objects that need to be instatiated in an async function.
The easiest way to do this is subscribing a method on your model to `hikari.StartingEvent`.

```python
class Model:
    def __init__(self) -> None:
        self._db: Database | None = None

    async def on_start(self, _: hikari.StartedEvent) -> None:
        self._db = await Database.create()

    @property
    def db(self) -> Database:
        assert self._db
        return self._db

model = Model()

bot = hikari.GatewayBot("TOKEN")
client = crescent.Client(bot, model)

bot.event_manager.subscribe(hikari.StartedEvent, model.on_start)

bot.run()
```
