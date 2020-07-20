---
layout: post
title: (TF) Which Layer Do I Need?
categories: [Research/ML&DL]
tags: [ML&DL, MNIST]
---



Full code can be found at the following github repository.
- [Park's GitHub > How to choose layer](https://github.com/jhyun0919/deep_dive_into_tensorflow/blob/master/tutorials/MNIST/how%20to%20choose%20layer.ipynb)


This post was written with reference to the following materials.
- [Aymeric Damien's GitHub > Neural Network Example](https://github.com/aymericdamien/TensorFlow-Examples/blob/master/tensorflow_v2/notebooks/3_NeuralNetworks/neural_network.ipynb)
- [Aymeric Damien's GitHub > Convolutional Neural Network Example](https://github.com/aymericdamien/TensorFlow-Examples/blob/master/tensorflow_v2/notebooks/3_NeuralNetworks/convolutional_network.ipynb)
- [Irhum Shafkat's Medium posting > Intuitively Understanding Convolutions for Deep Learning](https://towardsdatascience.com/intuitively-understanding-convolutions-for-deep-learning-1f6f42faee1)
- [Aymeric Damien's GitHub > Recurrent Neural Network Example](https://github.com/aymericdamien/TensorFlow-Examples/blob/master/tensorflow_v2/notebooks/3_NeuralNetworks/recurrent_network.ipynb)
- [Park's SlideShare > Understanding LSTM](https://www.slideshare.net/JiHyunPark18/understanding-lstm-and-its-diagrams)


---
## Dense Model
<p align="center">
<script src="https://gist.github.com/jhyun0919/ff71e7f4416a2add611074ad39888eab.js"></script>
</p>


## CNN Model
<p align="center">
<script src="https://gist.github.com/jhyun0919/1043aeb7181b81ac0106d0bbcc0f74d3.js"></script>
</p>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-20-how to choose layer/cnn visualized.gif" width="60%" />
  <figcaption>Figure 1. Visualization of how convolutional neural network layer works.</figcaption>
</figure>


## RNN(LSTM) Model
<p align="center">
<script src="https://gist.github.com/jhyun0919/f6d3f2c79049c74b6a74b6b792e20ba7.js"></script>
</p>


## Experiment Result
<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-20-how to choose layer/tensorboard result.png" width="95%" />
  <figcaption>Figure 2. Checking the validation result with tensorboard.</figcaption>
</figure>

