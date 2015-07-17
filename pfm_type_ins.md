``` python
import time

import zope.interface


class IBase(zope.interface.Interface):
    """"""


@zope.interface.implementer(IBase)
class Base():
    """"""


class Child(Base):
    """"""


def test_ProvideBy():
    begin_time = time.time()
    base = Base()
    for each in range(10000000):
        IBase.providedBy(base)
    print("ProvideBy Use: %s s" % (time.time() - begin_time))


def test_implementedBy():
    begin_time = time.time()
    for each in range(10000000):
        IBase.implementedBy(Base)
    print("implementedBy Use: %s s" % (time.time() - begin_time))


def test_isinstance():
    begin_time = time.time()
    base = Base()
    for each in range(10000000):
        isinstance(base, Base)
    print("isinstance Use: %s s" % (time.time() - begin_time))


def test_issubclass():
    begin_time = time.time()
    for each in range(10000000):
        issubclass(Base, Base)
    print("issubclass Use: %s s" % (time.time() - begin_time))


def test_type():
    begin_time = time.time()
    base = Base()
    for each in range(10000000):
        type(base) == Base
    print("type Use: %s s" % (time.time() - begin_time))

test_ProvideBy()
test_implementedBy()
test_isinstance()
test_issubclass()
test_type()
```

- At mac book air
- ProvideBy Use: 22.134658098220825 s
- implementedBy Use: 17.605348110198975 s
- isinstance Use: 1.553006887435913 s
- issubclass Use: 2.8824241161346436 s
- type Use: 1.8422999382019043 s
