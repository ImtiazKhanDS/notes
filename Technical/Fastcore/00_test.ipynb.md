


```Python
def test_fail(f, msg='', contains='', args=None, kwargs=None):
    "Fails with `msg` unless `f()` raises an exception and (optionally) has `contains` in `e.args`"
    args, kwargs = args or [], kwargs or {}
    try: f(*args, **kwargs)
    except Exception as e:
        assert not contains or contains in str(e)
        return
    assert False,f"Expected exception but none raised. {msg}"
```

1. We can check that code raises an exception when that's expected (`test_fail`).
2. examples 

```Python
def _fail(): raise Exception("foobar")
test_fail(_fail, contains="foo")

def _fail(): raise Exception()
test_fail(_fail)
```


```Python
def test(a, b, cmp, cname=None):
    "`assert` that `cmp(a,b)`; display inputs and `cname or cmp.__name__` if it fails"
    if cname is None: cname=cmp.__name__
    assert cmp(a,b),f"{cname}:\n{a}\n{b}"
```

```Python
def is_iter(o):
    "Test whether `o` can be used in a `for` loop"
    #Rank 0 tensors in PyTorch are not really iterable
    return isinstance(o, (Iterable,Generator)) and getattr(o,'ndim',1)
```

3. ` __mro__`    is the method resolution order , As long as we have single inheritance `__mro__` is just the tuple of the class, its base and its base's base.

```Python
def isinstance_str(x, cls_name):
    "Like `isinstance`, except takes a type name instead of a type"
    return cls_name in [t.__name__ for t in type(x).__mro__]
```

4. In numpy if arrays `a`  = [1, 2] and `b` = [1, 2] , we can compare this two for equality as
    `(a == b).all()`
5. 