# [ImageNet Classification with Deep Convolutional Neural Networks](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf)

## Introduction

1. The network was trained on the subsets of ImageNet used in the ILSVRC-2010 and ILSVRC-2012
   competitions. Achieved top results.
2. The network has 60 million parameters and 650000 neurons.
3. Consists of 5 convolution layers, some of which are follwed by max-pooling layers and 3 fully
   connected layers with a final 1000-way softmax.
4. "Dropout" was used to prevent overfitting.
5. Anything less than 5 convolution layers and 3 fully connected layers gave inferior performance
   and higher depths were limited by cost _only_.

## Dataset

1. ImageNet had multiple resolution images, images were down-sampled to 256 by 256.

## Architecture

1. ReLU non-linearity was used in place of tanh, increased speed by six times for the same accuracy.
   The ReLU non-linearity is applied to the output of every convolutional and fully-connected layer.
2. The architecture

   ![arch](https://cdn-images-1.medium.com/max/1200/1*mTVOfTeUYxJnv0jedMLVwg.png)

3. Trained on multiple GPU's, not so important.
4. Local Response Normalization, not used anymore.
5. Overlapping Pooling, check [this](https://stats.stackexchange.com/questions/283261/why-does-overlapped-pooling-help-reduce-overfitting-in-conv-nets) and [this](https://adriancolyer.files.wordpress.com/2017/03/overlapping-pooling.jpeg) for more info.
6. The image size in the architecture chart should be (227 by 227) instead of (224 by 224), as was pointed out by Andrej Karpathy in his famous CS231n course.

## Reducing Overfitting

### Data Augmentation

1. Generating image translations and reflections.
2. Altering the intensities of the RGB channels in training images.

### Dropout

1. Dropout is used in the first two fully-connected layers with probability 0.5. Without dropout, the network exhibits substantial overfitting. Dropout roughly doubles the number of iterations required to converge.

## Learning Details

1. Models were trained using stochastic gradient descent
   with a batch size of 128 examples, momentum of 0.9, and
   weight decay of 0.0005.
2. Update Rule:

   ![ur](https://cdn-images-1.medium.com/max/1600/1*zRCEzN657yvGBXZGBoG2Jw.png)

3. An equal learning rate was used for all layers, which was adjusted manually throughout training.
   The heuristic was to divide the learning rate by 10 when the validation error rate stopped improving with the current learning rate. The learning rate was initialized at 0.01 and reduced three times prior to termination.
