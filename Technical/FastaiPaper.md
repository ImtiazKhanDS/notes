
#### Fastai Paper Highlights

1. Design Goals of Fastai
   1. approachable and rapidly productive
   2. hackable and configurable
2. Fastai wanted the development speed of Keras and customisability of PyTorch which has resulted into layered architecture Design

![Fastai Architecture](FastaiPaper-image-1.png)


3. Fastai provides a `single Learner class` which brings together architecture, optimiser, and data, and automatically chooses an appropriate loss function wherever possible.
4. Transfer learning is critically important for training models quickly, accurately, and cheaply, but the details matter a great deal,  fastai automatically provides transfer learning optimised batch normalisation, training, layer freezing, and discriminative learning rates.
5. The library is built on top of PyTorch , Numpy, PIL and Pandas and various other libraries.
6. Some of the tasks used in the paper are 
  1. Image Classification
  2. Image Segmentation
  3. Object Detection
  4. Sentiment Classification
  5. Tabular Data Classification
  6. Collaborative Filtering
7. The learning rate finder does a mock training with an exponentially growing learning rate over 100 iterations. A good value is then the minimum value on the graph divided by 10.
8. `Learner's 2-way callback system` allows gradients , data ,losses , control flow and anything else to be read and changed at any point during training.
9. Fastai’s callback system is the first that we are aware of that supports the design principles necessary for complete two-way callbacks:
   1. A callback should be available to every single point that code can be run during training, so that a user can customise every single detail of the training method;
   2. Every callback should be able to access every piece of information available at that stage in the training loop, including hyper-parameters, losses, gradients, input and target data and so forth;
   3. Every callback should be able to modify all these pieces of information , at any time before they are used, and be able to skip a batch, epoch, training or validation section or cancel the whole training loop.
10. `NBDEV` is a system for exploratory programming.