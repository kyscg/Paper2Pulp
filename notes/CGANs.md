# [Conditional Generative Adversarial Nets](https://arxiv.org/abs/1411.1784)

## Very Short Introduction

1. This paper introduces the conditional version of generative adversarial nets, which can be constructed by simply feeding the data, y, we wish to condition on to both the generator and discriminator.
2. Specifically, CGAN concatenates a one-hot vector y to the random noise vector z.
3. The objective function for the two-player mini-max game has conditional probabilities instead of the usual.
