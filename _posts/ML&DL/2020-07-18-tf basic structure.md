---
layout: post
title: (TF) Basic Steps for Training a Model
categories: [Research/ML&DL]
tags: [ML&DL, Tensorflow]
---

TesorFlow is one of the most well-known deep learning frameworks. This post will describe the basic steps for training a model with Tensorflow. Briefly, the basic steps for how to train a model through TensorFlow are as follow.

1. Load & pre-process the dataset
2. Set a model
3. Compile the model : set an optimizer, a loss function, & metrics
4. Fit the model
5. Evaluate the model

Full code can be found at the following github repository.
- [Park's GitHub > Basic Structure of Training a Model](https://github.com/jhyun0919/deep_dive_into_tensorflow/blob/master/tutorials/TF_Basic/basic%20structure.ipynb)


This post was written with reference to the following materials.
- [TensorFlow official website > TensorFlow 2 quickstart for beginners](https://www.tensorflow.org/tutorials/quickstart/beginner)
- [TensorFlow official website > TensorFlow 2 quickstart for experts](https://www.tensorflow.org/tutorials/quickstart/advanced)
- [TensorFlow official website > Get started with TensorBoard](https://www.tensorflow.org/tensorboard/get_started)

---
# 1. Load & pre-process the dataset

For the experiment to show the basic process of training a model with TensorFlow, we used the MNIST dataset. The given images are a grayscale, so we need a preprocessing process to scale the image data features from 0 to 1.

<p align="center">
<script src="https://gist.github.com/jhyun0919/8a18592350c64011bb603bc359f32691.js"></script>
</p>
<br/>

# 2. Set a model

In this step, we designed the structure of a model. The model in this experiment was relatively simple, so it could be easier to construct the model using [tf.keras.Sequential](https://www.tensorflow.org/api_docs/python/tf/keras/Sequential). However, we use custumized model for the experiment. This is because a custumized model must be used when constructing a more complex model in the future, and it is necessary to become familiar with it. More specifically, in case of the model becomes complicated and the input features do not flow sequentially, the model must be constructed in a custum manner. [Transformer model](https://www.tensorflow.org/tutorials/text/transformer) is a good example.

<p align="center">
<script src="https://gist.github.com/jhyun0919/36bc28aa8f6e9005494d40603cccd5e7.js"></script>
</p>
<br/>

## (optional) Set callbacks
## checkpoint

We set *checkpoint* callback to record the training process of the model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/e5a2c5bc5d9f9634c2d97ee93c998c52.js"></script>
</p>
<br/>

## tensorboard

We set *tensorboard* callback to visualize the training process and strucutre of the model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/b8688659e5d183e5d82863d0972fc16d.js"></script>
</p>
<br/>

## reduce learning rate

We set *reduce-learning-rate* callback for better training result of the model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/cfea4379c657f506dd3dbf7d4f0adc3d.js"></script>
</p>
<br/>

# 3. Compile the model
## Set optimizer, loss function, & metrics

After constructing the model, you need to set the learning process by calling the *compile method*. Compile method has three important parameters; *optimizer, loss, and metrics*.

The *optimizer* sets up the training process. The *loss* sets the loss function to be minimized during the optimization process. The *metrics* are used to monitor training. These should be set differently depending on the type of feature data and the purpose of the model.

The model was compiled using the set optimizer, loss, and metrics.

<p align="center">
<script src="https://gist.github.com/jhyun0919/0f16de0b5385716af46692a163dc51dd.js"></script>
<script src="https://gist.github.com/jhyun0919/a5546b9e76e294ecc1362665cca1d048.js"></script>
</p>
<br/>

# 4. Fit the model

We conducted training using the preprocessed dataset and the compiled model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/bbc01cdfc18a32a1afe05c30ef846ccf.js"></script>
</p>
<br/>

## (Optional) Check the training process via tensorboard

Through the tensorboard callback set in the previous step, we visualized the training process and the model graph. Figure 1 shows the training process of the model.

<p align="center">
<script src="https://gist.github.com/jhyun0919/e196847e8797b94e947fb43081c69014.js"></script>
</p>

<figure align="center">
  <img src="https://jhyun0919.github.io/assets/img/2020-07-19-tf basic structure/tensorboard eg.png" width="95%" />
  <figcaption>Figure 1. Checking the training process with tensorboard.</figcaption>
</figure>
<br/>

## (Optional) Load the best weights

Through the checkpoint callback set in the previous step, we loaded the weights showing the best performance.

<p align="center">
<script src="https://gist.github.com/jhyun0919/e08262f64e42a6897de8cb360a7f3533.js"></script>
</p>
<br/>

# 5. Evaluate the model

As a final step, we evaluated the performance of the model using the test dataset.

<p align="center">
<script src="https://gist.github.com/jhyun0919/b83301942dc8f6ac7facbeafe47c0eb4.js"></script>
</p>