# [Visualizing and Understanding Convolutional Networks](https://arxiv.org/abs/1311.2901)

## Introduction

![ZFNet](https://cdn-images-1.medium.com/max/1200/1*bFjBVvUL2Po_p2mKzC4iYQ.png)

1. Introduction of a novel visualization technique that gives insight into the function of intermediate feature layers and the operation of the classifier.
2. Without clear understanding of how and why deep ConvNets work, the development of better models is reduced to trial-and-error. In this paper we observe a visualization technique that reveals the input stimuli that excite individual feature maps at any layer in the model. It also allows us to observe the evolution of features during training and to diagnose potential problems with the model.

## Visualizing with a DeConvNet

1. As we know, a standard step in most deep learning frameworks is to have a series of **Conv > Rectification (Activation Function) > Pooling**. To visualize a deep layer feature, we need a set of DeConvNet techniques to reverse the above actions such that we can visualize the feature in pixel domain.

    ![DeConvolution Process](https://cdn-images-1.medium.com/max/1200/1*aph2aB6IcCuMft1-MLqtqQ.png)

2. We need to map these activities back to the input pixel space, showing what input pattern originally caused a given activation in the feature maps. We use a DeConvNet for this purpose.
3. A DeConvNet can be thought of as a convnet model that uses the same components (filtering, pooling) but in reverse, so instead of mapping pixels to features does the opposite.
4. To examine a convnet, a DeConvNet is attached to each of its layers, as illustrated in the above figure, providing a continuous path back to image pixels. To examine a given ConvNet activation, we set all other activations in the layer to zero and pass the feature maps as input to the attached DeConvNet layer. Then we successively (i) unpool, (ii) rectify and (iii) filter to reconstruct the activity in the layer beneath that gave rise to the chosen activation. This is then repeated until input pixel space is reached.
5. Unpooling, Rectification and DeConvolution are shown below:

![Unpooling](https://cdn-images-1.medium.com/max/1200/1*KyfQTpv1hYDg8ABXNt0FVg.jpeg)

![Convolution](https://cdn-images-1.medium.com/max/1200/0*uxsQQN6UtlxksaDX)

![DeConvolution](https://cdn-images-1.medium.com/max/1200/0*CJYLcAXhmOepbMmh)

## Training Details

1. Architecture used is similar to [AlexNet](https://github.com/kyscg/Paper2Pulp/blob/master/notes/AlexNet.md)
2. One difference is that the sparse connections used in Krizhevsky’s layers 3,4,5 (due to the model being split across 2 GPUs) are replaced with dense connections in our model.
3. Stochastic gradient descent with a mini-batch size of 128 was used to update the parameters, starting with a learning rate of 10<sup>−2</sup> , in conjunction with a momentum term of 0.9.
4. We anneal the learning rate throughout training manually when the validation error plateaus.
5. Dropout is used in the fully connected layers (6 and 7) with a rate of 0.5.
6. All weights are initialized to 10<sup>−2</sup> and biases are set to 0.

## ConvNet Visualization

1. By using DeConvolution techniques, the top 9 activated patterns in randomly selected feature maps are shown for each layer. And two problems are observed in layer 1 and layer 2.

    ![Layer 1 and Layer 2](https://cdn-images-1.medium.com/max/1200/1*WbyE9tqJt8Kd0vqNX9MeVQ.png)

2. **Filters at layer 1 are a mix of extremely high and low frequency information**, with little coverage of the mid frequencies. Without the mid frequencies, there is a chain effect that deep features can only learn from extremely high and low frequency information.
3. **Layer 2 shows aliasing artifacts** caused by the large stride 4 used in the 1st layer convolutions. **Aliasing occurs when sampling frequency is too low.**

    ![Layer 3](https://cdn-images-1.medium.com/max/1200/1*hpm0NDbqDDTYHHY_7OOPfQ.png)

4. **Layer 3 starts to learn some general patterns**, such as mesh patterns, and text pattern.

    ![Layer 4 and Layer 5](https://cdn-images-1.medium.com/max/1200/1*69ty1ZX7OoScp7oXhqbs_A.png)

5. **Layer 4 shows significant variation, and is more class-specific**, such as dogs’ faces and birds’ legs.
6. **Layer 5 shows entire objects with significant pose variation**, such as keyboards and dogs.
