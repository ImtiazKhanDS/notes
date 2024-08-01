
1. Inspiration from good practices from various languages
	1. multiple dispatch from Julia
	2. mixins from Ruby
	3. currying, binding from Haskell
2. A tour of fastcore features
	1. docs
	2. testing
	3. [Mixins](https://en.wikipedia.org/wiki/Mixin) are a language concept that allows a programmer to inject some code into a class.
	4. Uses mixins concept to attach new behaviour to existing libraries , example `path.ls` which lists a directory and returns an list.
	5. You can easily add you own mixins with the [`patch`](https://fastcore.fast.ai/basics.html#patch) [decorator](https://realpython.com/primer-on-python-decorators/), which takes advantage of Python 3 [function annotations](https://www.python.org/dev/peps/pep-3107/#parameters) to say what class to patch:
	

```Python
	p = Path('images')
	p.ls()
	
	
	@patch
	def num_items(self:Path): return len(self.ls())
	p.num_items()
	#prints number of files in the current directory
	```


3. We also use `**kwargs` frequently. In python `**kwargs` in a parameter like means “_put any additional keyword arguments into a dict called `kwargs`_”. Normally, using `kwargs` makes an API quite difficult to work with, because it breaks things like tab-completion and popup lists of signatures. `utils` provides [`use_kwargs`](https://fastcore.fast.ai/meta.html#use_kwargs) and [`delegates`](https://fastcore.fast.ai/meta.html#delegates) to avoid this problem. See our [detailed article on delegation](https://www.fast.ai/2019/08/06/delegation/) on this topic.
4. [`GetAttr`](https://fastcore.fast.ai/basics.html#getattr) solves a similar problem (and is also discussed in the article linked above): it’s allows you to use Python’s exceptionally useful [`__getattr__`](https://fastcore.fast.ai/xml.html#__getattr__) magic method, but avoids the problem that normally in Python tab-completion and docs break when using this. For instance, you can see here that Python’s `dir` function, which is used to find the attributes of a python object, finds everything inside the `self.default` attribute here:

```Python
class Author:
	def __init__(self, name): self.name = name

class ProductPage(GetAttr):
	_default = 'author'
	def __init__(self,author,price,cost): self.author,self.price,self.cost = author,price,cost

p = ProductPage(Author("Jeremy"), 1.50, 0.50)
[o for o in dir(p) if not o.startswith('_')]
```

5. 