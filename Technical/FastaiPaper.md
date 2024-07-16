
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
8. 
