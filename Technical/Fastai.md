


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

   1. **torch.backends.cudnn.benchmark = True** , This enables benchmark mode in           cudnn.benchmark mode is good whenever your input sizes for your network do not vary. This way, cudnn will look for the optimal set of algorithms for that particular configuration (which takes some time). This usually leads to faster runtime. But if your input sizes changes at each iteration, then cudnn will benchmark every time a new size appears,          possibly leading to worse runtime performances.
   2. Sets a default gpu device.

2. subplots

    ```Python 
    @delegates(plt.subplots, keep=True)
def subplots(
    nrows:int=1, # Number of rows in returned axes grid
    ncols:int=1, # Number of columns in returned axes grid
    figsize:tuple=None, # Width, height in inches of the returned figure
    imsize:int=3, # Size (in inches) of images that will be displayed in the returned figure
    suptitle:str=None, # Title to be set to returned figure
    **kwargs
) -> (plt.Figure, plt.Axes): # Returns both fig and ax as a tuple
    "Returns a figure and set of subplots to display images of `imsize` inches"
    if figsize is None:
        h=nrows*imsize if suptitle is None or imsize>2 else nrows*imsize+0.6 #https://github.com/matplotlib/matplotlib/issues/5355
        figsize=(ncols*imsize, h)
    fig,ax = plt.subplots(nrows, ncols, figsize=figsize, **kwargs)
    if suptitle is not None: fig.suptitle(suptitle)
    if nrows*ncols==1: ax = array([ax])
    return fig,ax
    ```

   1. The decorator @delegates will pass down keyword arguments from plt.subplots to subplots function , so that you can use shift-tab to see all the parameters of plt.subplots while calling subplots
   2. figsize is determined by nrows and ncols, very briefly figsize = (ncols * imsize, nrows * imsize)
   3. Returns fig and ax , fig is the actual figure , ax is a array which helps to go into each section of figure.

References

1. fast core top features :  https://fastpages.fast.ai/fastcore/
2. @delegates problem and solution : https://www.fast.ai/posts/2019-08-06-delegation.html