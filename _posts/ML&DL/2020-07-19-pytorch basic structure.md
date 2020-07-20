---
layout: post
title: (PyTorch) Basic Structure
categories: [Research/ML&DL]
tags: [ML&DL, PyTorch]
---
PyTorch is one of the most well-known deep learning frameworks as well as Tensorflow. This post will describe the basic sturcture of traing a model with PyTorch. Briefly, the basic structure of how to train a model is as follows.

Full code can be found at the following git repository.
- [Park's GitHub > Basic Structure of Training a Model](https://github.com/jhyun0919/deep_dive_into_pytorch/blob/master/tutorials/01.%20basic/pytorch%20basic%20structure.ipynb)

This post was written with reference to the following materials.
- [JiHyung Moon's Meduim Posting](https://medium.com/@inmoonlight/pytorch%EB%A1%9C-%EB%94%A5%EB%9F%AC%EB%8B%9D%ED%95%98%EA%B8%B0-intro-afd9c67404c3)
- [PyTorch official webpage > Tutorials > Visualizing Models, Data, and Training with TensorBoard](https://pytorch.org/tutorials/intermediate/tensorboard_tutorial.html)

---

## 1. Load & pre-process the dataset
<p align="center">
<script src="https://gist.github.com/jhyun0919/6bd4ac356c46bfc7efe42e664ab83403.js"></script>
</p>

## 2. Set up a model
<p align="center">
<script src="https://gist.github.com/jhyun0919/5023e2b1d56c0fe89961ba09ef192476.js"></script>
</p>

## 3. Set optimizer & loss function
<p align="center">
<script src="https://gist.github.com/jhyun0919/e916c7736f96b4c5df111d81bece262e.js"></script>
</p>


## 4. Train the model

1. init the Gradient
2. forward propagation
3. calculate the loss
4. backward propagation
5. optimize (update) the weights

<p align="center">
<script src="https://gist.github.com/jhyun0919/93422cef8d6be9df23b2c8ecc55dc918.js"></script>
</p>

## 5. Evaluate the model
<p align="center">
<script src="https://gist.github.com/jhyun0919/5cb29e6e63d6c0a68a70742b2a1eba9d.js"></script>
</p>

