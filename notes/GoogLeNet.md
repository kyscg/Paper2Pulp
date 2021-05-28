# [Going Deeper With Convolutions](https://arxiv.org/abs/1409.4842)

## Introduction

1. As of recent years, efficiency of algorithms is important and hence, the considerations leading to this design included this factor rather than only accuracy
   numbers.
2. Computational budget of "1.5 billion multiply-adds" at inference time so that it can be used for practical purposes.
3. Reference to the meme, "We need to go deeper"

   ![meme](https://cdn-images-1.medium.com/max/1600/0*9oltTOaaHAbeLzeh.jpg)

4. From LeNet, the trend has been to increase the number of layers and layer size while using dropout to prevent overfitting.
5. A straightforward way to improve performance od deep nets looks to be increasing their size, both depth and width.

   a. Issue 1: Loads of parameters will be requires -> prone to overfitting -> requirement of more strongly labelled data -> laborious and expensive ->
   often requiring human raters.

   b. Issue 2: Increased compute, uniform increase in number of filters results in quadratic increase of compute. If gradients vanish, most of the compute
   will be wasted.

6. "As computational budget is finite, efficient resource distribution is better than increasing size indiscriminately.
7. Solution: Introduce sparsity and replace the fully connected layers by the sparse ones, even inside the convolutions. This is mathematically supported too and
   resonates with the well known Hebbian principle.

## Usage of the 1 \* 1 convolutions

1. The introducers and the initial users of the 1 by 1 convolutions used it to introduce non-linearity to increase the representational power.
2. In GoogLeNet, we use it for dimensionality reduction purposes and thereby, reducing computation. When we do so, we can increase the depth and width.

## Inception Module

1. Without 1 by 1 convolution:

   ![con](https://cdn-images-1.medium.com/max/1600/1*m1wn5P5BFZydFgVd3RiZNw.png)

2. Using dimensionality reduction:

   ![dred](https://cdn-images-1.medium.com/max/1600/1*sezFsYW1MyM9YOMa1q909A.png)

## Global Average Pooling

1. In previous nets like AlexNet, we connected all inputs to outputs at the end for the fully-connected layers, here, we use global average pooling as shown below.

   ![pool](https://cdn-images-1.medium.com/max/1600/1*0-wMHcASLDFzx9YBRCZXHg.png)

2. Originally, the number of weights were 7*7*1024\*1024, but with global average pooling, we have no weights to learn.

## Architecture

![arch](https://cdn-images-1.medium.com/max/2600/1*ZFPOSAted10TPd3hBQU8iQ.png)

Details of the parameters:

![para](https://cdn-images-1.medium.com/max/1600/1*lRN3h9a_qJdT6NIy0VOu3Q.png)

## Intermediate Softmax Classifiers

1. These are just used as auxiliary classifiers which have
   a. 5 by 5 average pooling with stride 3
   b. 1 by 1, 128 convolution filters
   c. 1024 Fully Connected
   d. 1000 Fully Connected
   e. Softmax
   f. Loss added to the total loss with weight 0.3
2. Used against vanishing gradient problem and provide regularization.
3. **Not used in test or inference time**
