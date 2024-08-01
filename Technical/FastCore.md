
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
	6. patch
	7. GetAttr
	8. store_attr
	9. basic_repr
	10. function_kwargs
	11. ProcessPoolExecutor
	12. `L` a replacement for `list`
	13. Transform
3. More Details here  https://fastcore.fast.ai/tour.html