# [Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks](https://arxiv.org/abs/1511.06434)

## Introduction

1. I already have a good idea about this from Dr. Goodfellow's NIPS 2016 tutorial, so the paper should be relatively easy to go through. This means that this file while be short and way less understandable.
2. This networks generates pictures of bedrooms.
3. Main contributions of the paper:
   - The authors propose and evaluate a set of constraints on the architectural topology of Convolutional GANs that make them stable to train in most settings. This class of architectures is called DCGANs.
   - We use the trained discriminators for image classification tasks, showing competitive performance with other unsupervised algorithms.
   - We visualize the filters learnt by GANs and empirically show that specific filters have learned to draw specific objects.
   - We show that the generators have interesting vector arithmetic properties allowing for easy manipulation of many semantic qualities of generated samples.

## Architecture for stable DCGANs

1. Replace any pooling layers with strided convolutions (discriminator) and fractional-strided convolutions (generator).
2. Use batchnorm in both the generator and the discriminator.
3. Remove fully connected hidden layers for deeper architectures.
4. Use ReLU activation in generator for all layers except for the output, which uses Tanh.
5. Use LeakyReLU activation in the discriminator for all layers.

![DCGAN generator used for LSUN scene modeling](https://cdn-images-1.medium.com/max/1200/1*rdXKdyfNjorzP10ZA3yNmQ.png)

## ReLU vs. LeakyReLU

1. The ReLU activation function will just take the maximum between the input value and zero. If we use the ReLU activation function, sometimes the network gets stuck in a popular state called the **dying state**, and that’s because the network produces nothing but zeros for all the outputs.
2. Leaky ReLU prevent this dying state by allowing some negative values to pass through. The whole idea behind making the Generator work is to receive gradient values from the Discriminator, and if the network is stuck in a dying state situation, the learning process won’t happen.
3. The output of the Leaky ReLU activation function will be positive if the input is positive, and it will be a controlled negative value if the input is negative. Negative value is control by a parameter called **alpha**, which will introduce tolerance of the network by allowing some negative values to pass.

## Training of DCGANs

1. First the Generator creates some new examples.
2. The Discriminator is trained using real data and generated data.
3. After the Discriminator has been trained, both models are trained together.
4. The Discriminator’s weights are frozen, but its gradients are used in the Generator model so that the Generator can update it’s weights.

## [Up-sampling with Transposed Convolution](https://towardsdatascience.com/up-sampling-with-transposed-convolution-9ae4f2df52d0)
