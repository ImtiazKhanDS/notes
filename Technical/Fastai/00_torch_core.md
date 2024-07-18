

### Code Reading

1. **setup_cuda**

	``` python
def setup_cuda(benchmark=defaults.benchmark):
    "Sets the main cuda device and sets `cudnn.benchmark` to `benchmark`"
    if torch.cuda.is_available():
        if torch.cuda.current_device()==0:
            def_gpu = int(os.environ.get('DEFAULT_GPU') or 0)
            if torch.cuda.device_count()>=def_gpu: torch.cuda.set_device(def_gpu)
        torch.backends.cudnn.benchmark = benchmark 
```

	1. **torch.backends.cudnn.benchmark = True** , This enables benchmark mode       in           cudnn.benchmark mode is good whenever your input sizes for your network do not vary. This way, cudnn will look for the optimal set of algorithms for that particular configuration (which takes some time). This usually leads to faster runtime. But if your input sizes changes at each iteration, then cudnn will benchmark every time a new size appears, possibly leading to worse runtime performances.
	2. Sets a default gpu device.

2. **subplots**

    ``` python 
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

    1. The decorator `@delegates` will pass down keyword arguments from plt.subplots to subplots function , so that you can use shift-tab to see all the parameters of plt.subplots while calling subplots
    2. figsize is determined by nrows and ncols, very briefly `figsize = (ncols * imsize, nrows * imsize)`
    3. Returns `fig` and `axs` , fig is the actual figure , `axs` is a array which helps to go into each section of figure.

3. **fig_bounds**

   ``` python
   def _fig_bounds(x):
    r = x//32
    return min(5, max(1,r))
```

    1.  bounds an image used in show_image function below.

4. **show_image**

   ``` python
@delegates(plt.Axes.imshow, keep=True, but=['shape', 'imlim'])
def show_image(im, ax=None, figsize=None, title=None, ctx=None, **kwargs):
    "Show a PIL or PyTorch image on `ax`."
    # Handle pytorch axis order
    if hasattrs(im, ('data','cpu','permute')):
        im = im.data.cpu()
        if im.shape[0]<5: im=im.permute(1,2,0)
    elif not isinstance(im,np.ndarray): im=array(im)
    # Handle 1-channel images
    if im.shape[-1]==1: im=im[...,0]

    ax = ifnone(ax,ctx)
    if figsize is None: figsize = (_fig_bounds(im.shape[0]), _fig_bounds(im.shape[1]))
    if ax is None: _,ax = plt.subplots(figsize=figsize)
    ax.imshow(im, **kwargs)
    if title is not None: ax.set_title(title)
    ax.axis('off')
    return ax
```

    1. **hasattrs** tests whether im contains all attributes `data , cpu and permute` which handles PyTorch axis order.
    2. handles one channel images im=im[...,0] so here if the image dimension is 28 * 28 * 1 , then ellipsis followed by 0 makes it 28 * 28

5. **show_titled_image** 

   ``` python
@delegates(show_image, keep=True)
def show_titled_image(o, **kwargs):
    "Call `show_image` destructuring `o` to `(img,title)`"
    show_image(o[0], title=str(o[1]), **kwargs)
```

   
    1. ```{Python}show_titled_image((im, 'A puppy'), figsize =(2,2)```  provide image and title to show titled image.

6. **show_images** 

	``` python
@delegates(subplots)
def show_images(ims, nrows=1, ncols=None, titles=None, **kwargs):
    "Show all images `ims` as subplots with `rows` using `titles`."
    if ncols is None: ncols = int(math.ceil(len(ims)/nrows))
    if titles is None: titles = [None]*len(ims)
    axs = subplots(nrows, ncols, **kwargs)[1].flat
    for im,t,ax in zip(ims, titles, axs): show_image(im, ax=ax, title=t)
```


	1. subplots function returns a numpy array on which we can use .flat attribute to get each axs in a list and iterate over it to send that ax to show_image function for plotting.

7. ArrayBase

	``` python
class ArrayBase(ndarray):
    "An `ndarray` that can modify casting behavior"
    @classmethod
    def _before_cast(cls, x):
     return x if isinstance(x,ndarray) else array(x)
```

	1. **@classmethod** : A classmethod() is a built-in function in Python that is used _to define a method that is bound to the class and not the instance of the class_. class methods are aware of the class state.
	2. An `instance method`, simply put, is a function defined inside of a class. It varies with the different instances of the class, Whereas a `class method` is a method which, unlike the instance method, is applied to all instances of the class.


8. ArrayImageBase

   ``` python
class ArrayImageBase(ArrayBase):
    "Base class for arrays representing images"
    _show_args = {'cmap':'viridis'}
    def show(self, ctx=None, **kwargs):
        return show_image(self, ctx=ctx, **{**self._show_args, **kwargs})
    ```

	1. Base class for representing images
	2. 

**References**

1. fast core top features :  https://fastpages.fast.ai/fastcore/
2. @delegates problem and solution : https://www.fast.ai/posts/2019-08-06-delegation.html