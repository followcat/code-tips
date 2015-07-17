# before
I found an useful python keywork: nonlocal
``` python
    def make_toggle_button():
        state = False
        def _():
            nonlocal state
            state = not state
            return state
        return _

    >>> button_press = make_toggle_button()
    >>> button_press()
    True
    >>> button_press()
    False
    >>> button_press()
    True
```

And another yield way:
``` python
    def toggle_button():
        while True:
            yield True
            yield False

    >>> button_press = toggle_button()
    >>> next(button_press)
    True
    >>> next(button_press)
    False
    >>> next(button_press)
    True
```

# Knowledge about Metaprogramming(2)
The appropriate metaclass is determined by the following precedence rules: 
If dict['__metaclass__'] exists, it is used. 
Otherwise, if there is at least one base class, its metaclass is used (this looks for a __class__ attribute first and if not found, uses its type). 
Otherwise, if a global variable named __metaclass__ exists, it is used. 
Otherwise, the old-style, classic metaclass (types.ClassType) is used. 

## Design Singleton with Meta
Here is a Singleton design with Meteclass:
``` python
    class Singleton(type):
        def __init__(self, name, bases, namespace):
            self._obj=type(name,bases,namespace)()

        def __call__(self):
            return self._obj


    class Cache(object):
        """
        >>> object() is object()
        False
        >>> Cache() is Cache()
        True
        """
        __metaclass__ = Singleton
```
But...The test can only pass in Python2.

Why?

## __metaclass__ only in Python2
In python3k, class Cache should be like this:
``` python
    class Cache(metaclass=Singleton):
        """
        >>> object() is object()
        False
        >>> Cache() is Cache()
        True
        """
```
Test pass in Python3.
