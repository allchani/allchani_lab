---
title: "[Udemy02] note01. CNN Theory part one" 
excerpt: "Complete Guide to TensorFlow for Deep Learning with Python"
mathjax : True
tags : 
    - Udemy02
---

- Just like the simple perceptron, CNNs also have their origins in biological research.
- Hubel and Wiesel studied the structure of the visual cortex in mammals, winning a Nobel Prize in 1981.
- Their research revealed that neurons in the visual cortex had a small __local receptive field__.
- This idea then inspired an ANN architecture that would become CNN
- Famously implemented in the 1998 paper by Yann LeCun et al.
- The LeNet-5 architecture was first used to classify the MNIST data sets.

---

- Tensors
- DNN vs CNN
- Convolutions and Filters
- Padding
- Pooling Layers
- Review Dropout

---
### Tensors
- Recall that Tensors are N-Dimensional Arrays that we build up to:
    + Scalar - 3
    + Vector - [3, 4, 5]
    + Matrix - [[3,4], [5,6], [7.8]]
    + Tensor - [[[1,2], [3,4]], [[5.6],[7,8]]]

- Tensors make it very convenient to feed in sets of images into our model - (I, H, W, C)
    + I : Images
    + H : Height of Image in Pixels
    + W : Width of Image in Pixels
    + C : Color Channels : 1-Grayscale, 3-RGB

- Now let's explore the difference between a Densely Connected Neural Network and a Convolutional Neural Network.
- Recall that we've already been able to create DNNs with tf.estimator API.


- Densely Connected layer
    + every neuron in one layer is directly connected to every other neuron in the next layer.
- Convolutional layer
    + Each unit is connected to a smaller number of nearby units in next layer.
    + idea of the biological visual cortex where you're only looking at local receptive fields.
- So why bother with a CNN instead of a DNN?
    + The MINIST dataset was 28 by 28 pixels(784 total)
    + But most images are at least 256 by 256 or greater,(<56k total)
    + This leads to too many parameters. unscalabe to new images.
        * Convolutions also have a major advantage for image processing, where pixels nearby to each other are much more correlated to each other for image detection.