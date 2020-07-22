---
layout: post
title: (PyTorch) Basic Steps for Training a Model
categories: [Research/ML&DL]
tags: [ML&DL, PyTorch]
---
PyTorch is one of the most well-known deep learning frameworks as well as Tensorflow. This post will describe the basic steps for traing a model with PyTorch. Briefly, the basic steps of how to train a model are as follow.

1. Load & pre-process the dataset
2. Set a model
3. Set an optimizer & a loss function
4. Train the model
5. Evaluate the model

Full code can be found at the following github repository.
- [Park's GitHub > Basic Structure of Training a Model](https://github.com/jhyun0919/deep_dive_into_pytorch/blob/master/tutorials/01.%20basic/pytorch%20basic%20structure.ipynb)

This post was written with reference to the following materials.
- [JiHyung Moon's Meduim Posting](https://medium.com/@inmoonlight/pytorch%EB%A1%9C-%EB%94%A5%EB%9F%AC%EB%8B%9D%ED%95%98%EA%B8%B0-intro-afd9c67404c3)
- [PyTorch official webpage > Tutorials > Visualizing Models, Data, and Training with TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_tutorial.html)

---

# 1. Load & pre-process the dataset

We used the *FashionMNIST* dataset for the experiment. The dataset can be downloaded through *torchvision.datasets*, and preprocessing can be performed through *transforms.Compose*.

<p align="center">
<script src="https://gist.github.com/jhyun0919/6bd4ac356c46bfc7efe42e664ab83403.js"></script>
</p>
<br/>

# 2. Set a model

We constructed a simple convolutional neural network model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/5023e2b1d56c0fe89961ba09ef192476.js"></script>
</p>
<br/>

# 3. Set optimizer & loss function

We set the optimizer and loss function to train the model constructed in the previous step. The optimizer and loss function types were selected according to the type of the features of the given input data and the purpose of the model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/e916c7736f96b4c5df111d81bece262e.js"></script>
</p>
<br/>

# 4. Train the model

In order to train a model, we must follow the following 5 steps.

1. initialize the gradient
2. forward propagation
3. calculate the loss
4. backward propagation
5. optimize (update) the weights based on forward & backward propagation

<p align="center">
<script src="https://gist.github.com/jhyun0919/93422cef8d6be9df23b2c8ecc55dc918.js"></script>
</p>
<br/>

# 5. Evaluate the model

We used the validation dataset to measure the performance of the completed training model. Note that the evaluate process is very similar to the training process. However, in the evaluate process, *torch.no_grad()* is required because the weights must never change.

<p align="center">
<script src="https://gist.github.com/jhyun0919/5cb29e6e63d6c0a68a70742b2a1eba9d.js"></script>
</p>

