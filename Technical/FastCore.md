
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
	6. 

```Python
	p = Path('images')
	p.ls()
	
	
	@patch
	def num_items(self:Path): return len(self.ls())
	p.num_items()
	#prints number of files in the current directory
	```