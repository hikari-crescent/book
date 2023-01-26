# Command Groups

Can commands can be grouped or grouped into groups of groups.
In Crescent these groups are called `groups` and `sub_groups`.


```python
import crescent

# Create a group
group = crescent.Group("outer-group")
# Create a sub group
sub_group = group.sub_group("inner-group")
```

To add a command to a group simply do:

```python
@client.include
@group.child
@crescent.command
async def group_command(ctx: crescent.Context):
    ...

@client.include
@sub_group.child
@crescent.command
async def sub_group_command(ctx: crescent.Context):
    ...
```

Do not combine the `group` and `sub_group` decorators. This will cause a command to be
registered multiple times.

### Illegal Combos

You can not create a group with the same name as a command.
```python
help_group = crescent.Group("help")

@client.include
@help_group.child
@crescent.command
async def say(ctx: crescent.Context):
    ...

# This command will cause the bot to crash
@client.include
@crescent.command
async def help(ctx: crescent.Context):
    ...
```
