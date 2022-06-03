
# Quickstart

This initial example shows first pattern matching and then dependency
resolution.

```python
from interlinked import run, provide, depend

@provide('echo')
@provide('echo.{name}')
def echo(name='default'):
    return name

@depend(value='echo.test')
@provide('many_echo')
def many_echo(value, repeat=2):
    return ' '.join([value] * repeat)


result = run("echo.spam")
print(result)  # -> spam

result = run("many_echo", repeat=4)
print(result)  # -> test test test test

```


This second example shows custom dependency resolution (based on
previous code).


```python

from interlinked.workflow import default_workflow as wkf

wkf.resolve = lambda target, **kw: wkf.run(target, **kw).upper()
print(wkf.run("many_echo"))  # -> TEST TEST
```

As you can see, we rebind the `resolve` method of the workflow.

This workflow object is instanciated for us when interlinked is
imported. This object is responsible to keep track of dependencies
(based on `depend` decorator) and to run our code (based on `provide`
decorator).

Resolve is the method that is invoked by the workflow to reify the
parameters defined in `depend`, by default it simply runs the
corresponding function and reuse the returned values as input.

By rebinding the `resolve` method we can inject custom logic at each
step of the workflow.


# Advanced usages

## Caching

The examples in the previous section just run the different functions
in cascade, just like a normal call stack. We can implement caching in
the resole step. See <examples/caching.py>.

## Multi workflow

More workflow objects can be created to support more complex
scenario. (TODO)
