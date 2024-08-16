


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

