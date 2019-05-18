# [Deep Residual Learning for Image Recognition](https://arxiv.org/pdf/1512.03385.pdf)

## Motivation:

1. Driven by the "depth" question in networks, we wonder if stacking loads of layers will
result in better learning.
2. First, there was the vanishing/exploding gradients problem but this was solved by pre-
initialization and intermediate normalization layers.
3. Now, there is a degradation problem. With the depth of the net increasing, accuracy gets
saturated and degrades with increasing depth. Below are some test results from the original
paper.

![](https://raw.githubusercontent.com/rohan-varma/resnet-implementation/master/images/verydeep_network.png)

## Solution:

1. Intuitively, deep networks should’t be “harder” to fit. If there is a certain number of layers N that achieve optimal accuracy on a dataset, then the layers after N could just learn the identity mapping (i.e. each layer computes their mapping as H(x)=x where H(x) is the mapping of the layer to be learned), and then the network will effectively have their final output at layer N.
2. The degradation problem suggests that the solvers might have difficulties in approximating identity mappings by multiple nonlinear layers. 
3. Authors introduce the idea of **residual learning** - instead of directly approximating the underlying mapping we want, H(x), we instead learn a residual function H(x)−x. This is done by making the output of a stack of layers be y=F(x)+x, where F(x) is the output of the layers (before the ReLU of the last layer) and then the original input x is element-wise added:

![](https://raw.githubusercontent.com/rohan-varma/resnet-implementation/master/images/residual_learning_block.png)

4. Therefore, if our underlying mapping is still y=H(x) that we want to learn, then F(x)=H(x)−x so that y=F(x)+x=H(x).
5. The idea of learning identity mappings is now easier, since we just need to set all weights to 0, so that H(x)=0 and F(x)=−x, so y=x is learned.
6. F(x)+x=H(x) can be realized using "shortcut connections" that perform identity mapping. These identity shortcut connections add neither extra parameters nor computational complexity.

## Deep Residual Learning
1. A building block in ResNet is defined as y=F(x<sub>i</sub>,W<sub>i</sub>) + x where F can be multiple layers. For example, the below figure has F=W<sub>2</sub>(σ(W<sub>1</sub>x)) unless we use a projection mapping to make the dimensions equal. The + operation is performed by a shortcut operation and element-wise addition.

![](https://cdn-images-1.medium.com/max/1600/1*37brTipLpo6naVYHiXMbsg.png)

![](https://cdn-images-1.medium.com/max/1600/1*07wrOB82Ktl3uWhY0GWE7A.png)

## Bottleneck Layer

1. In deeper variants of ResNet, bottleneck layers are used similar to that in GoogLeNet. The input is first fed through a 1 × 1 conv. layer to reduce the number of channels. After that, a 3 × 3 convolutions are performed followed by another 1 × 1 conv. layer to increase/restore the number of channels.

![](https://raw.githubusercontent.com/rohan-varma/resnet-implementation/master/images/bottleneck.png)

## Training

1. 224 × 224 crops are randomly sampled from an image resized such that its shorter side is randomly chosen form [256, 480], with the per-pixel mean subtracted. 
2. He initialization of weights is used, namely weights are initialized from sampling from a Gaussian with mean 0 and standard deviation √(2/n<sub>l</sub>). Biases are initialized to be 0.
3. Stochastic gradient descent with mini-batch size of 256, momentum of 0.9 and a weight decay of 0.0001 is used. The learning rate is initialized at 0.1 and is divided by 10 when the error plateaus. The models are trained for 60 × 10e4 iterations. 
4. Dropout is not used.
