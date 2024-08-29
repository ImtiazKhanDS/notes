


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
```Python
def array_equal(a,b):
	if hasattr(a, '__array__'): a = a.__array__()
	if hasattr(b, '__array__'): b = b.__array__()
	if isinstance_str(a, 'ndarray') and isinstance_str(b, 'ndarray'):  
		return (a==b).all() 
	return all_equal(a,b)
```

```Python
def any_is_instance(t, *args):
	return any(isinstance(a,t) for a in args)
```

5. sample example of using **any_is_instance**.

```Python
class temp:

	def __init__(self, a):
		self.a = a

a = temp(1)
b = 2
any_is_instance(temp, a,b)
 # This returns true because a is an instance of temp
a==b 
# This returns false because a is not equal to b
```


```Python
def equals(a,b):

"Compares `a` and `b` for equality; supports sublists, tensors and arrays too"
	if (a is None) ^ (b is None): return False
	
	if any_is_instance(type,a,b): return a==b
	
	if hasattr(a, '__array_eq__'): return a.__array_eq__(b)
	
	if hasattr(b, '__array_eq__'): return b.__array_eq__(a)
	
	cmp = (array_equal if isinstance_str(a, 'ndarray') or isinstance_str(b, 'ndarray') else
	
	array_equal if isinstance_str(a, 'Tensor') or isinstance_str(b, 'Tensor') else
	
	df_equal if isinstance_str(a, 'NDFrame') or isinstance_str(b, 'NDFrame') else
	
	operator.eq if any_is_instance((str,dict,set), a, b) else
	
	all_equal if is_iter(a) or is_iter(b) else
	
	operator.eq)

	return cmp(a,b)
```

6. Covers all conditions and chooses the cmp (comparator) as per the operands.

```Python
def all_equal(a,b):
	"Compares whether `a` and `b` are the same length and have the same   contents"

	if not is_iter(b): return a==b
	
	return all(equals(a_,b_) for a_,b_ in itertools.zip_longest(a,b))
```

7. 