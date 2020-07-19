---
layout: post
title: (TF) Basic Structure
categories: [Research/ML&DL]
tags: [ML&DL, Tensorflow]
---

This post will describe the basic sturcture of traing a model with Tensorflow. Briefly, the basic structure of how to train a model through Tensorflow is as follows.

- Data pre-process
- Set up a Model
- Set up an Optimizer, a Loss function, & Metrics
- Compile & Fit the Model

Full code can be found at the following git repository.
- [Basic Structure of Training a Model](https://github.com/jhyun0919/deep_dive_into_tensorflow/blob/master/tutorials/TF_Basic/basic%20structure.ipynb)

---
## 1. Load & pre-process the dataset
<p align="center">
<script src="https://gist.github.com/jhyun0919/8a18592350c64011bb603bc359f32691.js"></script>
</p>

## 2. Set up a model
<p align="center">
<script src="https://gist.github.com/jhyun0919/36bc28aa8f6e9005494d40603cccd5e7.js"></script>
</p>

## (optional) Set callbacks
### checkpoint
<p align="center">
<script src="https://gist.github.com/jhyun0919/e5a2c5bc5d9f9634c2d97ee93c998c52.js"></script>
</p>

### tensorboard
<p align="center">
<script src="https://gist.github.com/jhyun0919/b8688659e5d183e5d82863d0972fc16d.js"></script>
</p>

### reduce learning rate
<p align="center">
<script src="https://gist.github.com/jhyun0919/cfea4379c657f506dd3dbf7d4f0adc3d.js"></script>
</p>

## 3. Set optimizer, loss function, & metrics
<p align="center">
<script src="https://gist.github.com/jhyun0919/0f16de0b5385716af46692a163dc51dd.js"></script>
</p>

## 4. Compile & Fit the model
<p align="center">
<script src="https://gist.github.com/jhyun0919/a5546b9e76e294ecc1362665cca1d048.js"></script>
<script src="https://gist.github.com/jhyun0919/bbc01cdfc18a32a1afe05c30ef846ccf.js"></script>
</p>

## (Optional) Check the training process via tensorboard
<p align="center">
<script src="https://gist.github.com/jhyun0919/e196847e8797b94e947fb43081c69014.js"></script>
</p>

![picture 1](../../images/30cb46e50461d91ed9a6c3cd5dee984c92ab99a035c8be7de63f624cdddb08c4.png)<center>Figure 1. Checking the training process with tensorboard.</center>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-19-tf basic structure/tensorboard eg.png" width="100%" />
  <figcaption>Figure 1. Checking the training process with tensorboard.</figcaption>
</figure>

## (Optional) Load the best weights
<p align="center">
<script src="https://gist.github.com/jhyun0919/e08262f64e42a6897de8cb360a7f3533.js"></script>
</p>

## 5. Evaluate the model
<p align="center">
<script src="https://gist.github.com/jhyun0919/b83301942dc8f6ac7facbeafe47c0eb4.js"></script>
</p>