


### Function Explanations notebook wise

#### 00_torch_core.ipynb

##### Functions

1. setup_cuda

```Python
def setup_cuda(benchmark=defaults.benchmark):
    "Sets the main cuda device and sets `cudnn.benchmark` to `benchmark`"
    if torch.cuda.is_available():
        if torch.cuda.current_device()==0:
            def_gpu = int(os.environ.get('DEFAULT_GPU') or 0)
            if torch.cuda.device_count()>=def_gpu: torch.cuda.set_device(def_gpu)
        torch.backends.cudnn.benchmark = benchmark
```

1. `torch.backends.cudnn.benchmark = True` , This enables benchmark mode in cudnn.benchmark mode is good whenever your input sizes for your network do not vary. This way, cudnn will look for the optimal set of algorithms for that particular configuration (which takes some time). This usually leads to faster runtime. But if your input sizes changes at each iteration, then cudnn will benchmark every time a new size appears, possibly leading to worse runtime performances.

References

1. fast core top features :  https://fastpages.fast.ai/fastcore/
2. @delegates problem and solution : https://www.fast.ai/posts/2019-08-06-delegation.html
3. @delegates is a decorator is used to fix non appearing kwargs parameters to appear when you do shift tab in Jupyter or any repl environment.
4. This delegates function is part of fastcore library under meta.py