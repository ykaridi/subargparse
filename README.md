# subargparse
A python module for better handling subcommands in [argparse](https://github.com/bewest/argparse).

## Usage
First, you must create a decorator for decorating the commands
```python
import argparse
import subargparse
subparser = subargparse.subparser_decorator()
```

Next, you can configure your commands.
The command "path" should be passed in the decorator arguments (as a list of arguments), while the function name is the name of the command.<br/>
The function should define the parser argument as pleased.<br/>
The function returns a function which recieves the arguments object and handles it as pleased.

```python
@subparser("foo", "bar")
def test(parser):
  def action(args):
    print(args.text)
  parser.add_argument("text")
  return action
```
_In this example the command usage is<br/> 
foo bar [text]_

Now we must bind the subparser we created to our main parser.<br/>
That we do by binding the subparser decorator object to our vanilla parser

```python
parser = argparse.ArgumentParser()
subparser.bind(parser)
```

Last but not least, we handle the command the parser parsed.<br/>
```pyhton
args = parser.parse_args(["foo", "bar", "test", "Hello World!"])
parser.handle(args)
```


## Notes
It isn't possible to define a subparser in the "middle" of another subparser's path.<br/>
For example:
```python
@subparser("foo", "bar")
def test(parser):
  ...


@subparser("foo")
def bar(parser):
  ...
```
  
