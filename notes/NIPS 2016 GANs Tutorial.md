# NIPS 2016 Tutorial: Generative Adversarial Networks by Dr. Ian Goodfellow


I will not attempt to write a review, because the paper below is in itself a review, I'm merely storing this here for future reference.

## [YouTube: 2 hour watch](https://www.youtube.com/watch?v=HGYYEUSm-0Q)
## [ArXiv Summary Paper](https://arxiv.org/abs/1701.00160)
## [Tutorial Slides: 86 pages](http://www.iangoodfellow.com/slides/2016-12-04-NIPS.pdf)

## Some questions asked during the tutorial that I found interesting...

1. What prevents the generator from always generating the same image?

**Answer**: If we play the minimax game right, the generator would not be able to consistently fool the discriminator, the discriminator would learn to recognize that individual sample and reject it. Though, one of the failure modes of this model is that nearby models of a single sample will fool D while appearing the same to the human eye.

2. When should we use GANs and when should we use VAEs?

**Answer**: If our goal is to obtain a high likelihood, then we use VAE. But if we wanna generate more samples from the training distribution, we need to use GANs. The answer can also be obtained from looking at the individual cost functions.