# Knowledge about Metaprogramming(1)

Code-generating programs are sometimes called metaprograms; writing such programs is called metaprogramming.
Such as:
    Ruby            Reflection
    Lisp            macro
    Python          metaclass, exec
    JavaScript      eval
    Linux           lex, yacc
    C               #define
    ORM
    All Compiler

Metaprogram can run in compile time and runtime.

Let's begin with an interesting Python program:
First One:
    Entry checkmate Virtualenv
    > cd $CHECKMATE_HOME
    > ipython
    >>> exec(compile(__import__('zlib').decompress(
    __import__('base64').b64decode('eNp1U8FOwzAMvecrcmylKeKMtBsgIXHkNk1V1rojkCUhSdEQ4t+xk65NBvRQNfWz/fxezHotQ+APXp6gsYdX6GN7yzg+A4y865RRseuaAHrccGV6e1LmOCPooYDopdZ8u4TroJ80YPDrmy1l5TDQ37lq9NIEFZU1G+5s/grXLQi/i5PDrBUviJ2Sut1jgyWVFfThfSFv4wv4oqyHOHlTDrDNmHRijBXKPKkQG42vQpukzMrl1zBFq+g/1wM9IxVFzln2aqAs4e5m31YZauTGxswW5etShSa927p2WZ/gu5SjzADnGb+v8FkAawxanwHVFAsWzj24yB+p0L331tdtHYpVOTxTzLJcE8VxMkcaSmUX6nphcuCbVkjnwAwz8woxG/jsJ7g29UHqACuby3QFl3+cWi4RirdbdRqt5yD7F6JauGWneLToVs0cZ6doV5tM6e1v4VehqrQaubC6qFFYWqeteeksLpv2944xpk7OerxW8oSb1WF16qBVLwnAJF2hP0PiGUK8k1E2LRv1Zci0JvijUEvi1cIeBkwM4kPqCUIz602wA2EIK8AclQFx0LZ/C6ugoxbXi3ZA3mWLUWe488rEpHPaYGw8oM3pTBq07AfTUYNG')),'', 'exec'))

Another One:
    >>> exec(compile(__import__('zlib').decompress(
    __import__('base64').b64decode('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')),'', 'exec'))



In these two examples, use exec to define some function and then run it. (The magic string generate by zlib and base64 to reduce the code.)

## In Python

### Basic

    class C: pass

        is

    C = type('C', (), {})


### Metaclass's __call__

__call__ is called when the already-created class is "called" to instantiate a new object. 

``` python
    >>> class MyMeta(type):
    >>>     def __call__(cls, *args, **kwds):
    >>>         print '__call__ of ', str(cls)
    >>>         print '__call__ *args=', str(args)
    >>>         return type.__call__(cls, *args, **kwds)>>> 
    >>>
    >>> class MyKlass(object):
    >>>     __metaclass__ = MyMeta>>> 
    >>>
    >>>     def __init__(self, a, b):
    >>>         print 'MyKlass object with a=%s, b=%s' % (a, b)>>> 
    >>>
    >>> print 'gonna create foo now...'
    >>> foo = MyKlass(1, 2)
```