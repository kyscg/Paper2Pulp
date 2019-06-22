# [Explaining and Harnessing Adversarial Examples](https://arxiv.org/abs/1412.6572)

## Introduction

1. In [this paper](https://github.com/kyscg/Paper2Pulp/blob/master/notes/Intriguing%20Properties%20of%20Neural%20Networks.md), Szegedy et al. address some important issues related to SOTA neural networks. We learn that many nets are susceptible to adversarial attacks. That is, these machine learning models misclassify examples that are only slightly different from correctly classified examples drawn from the data distribution. And these attacks are transferable.
2. Speculative explanations have suggested it is due to extreme nonlinearity of deep neural networks, perhaps combined with insufficient model averaging and insufficient regularization of the purely supervised learning problem. But actually, linear behavior in high-dimensional spaces is sufficient to cause adversarial examples. This view enables us to design a fast method of generating adversarial examples that makes adversarial training practical.
3. Another point is that training on these adversarial examples provides additional regularization.
4. Changing to nonlinear model families like RBF networks help reduce the vulnerability to adversarial attacks.
5. This explanation suggests a fundamental tension between designing models that are easy to train due to their linearity and designing models that use nonlinear effects to resist adversarial perturbation. **Such an important line, really got me excited to read the entire paper, let's see how much longer before my excitement fades**.

## Important Conclusions from [Szegedy et al. (2014b)](https://github.com/kyscg/Paper2Pulp/blob/master/notes/Intriguing%20Properties%20of%20Neural%20Networks.md)

1. Box-constrained L-BFGS can reliably find adversarial examples.
2. On some datasets, such as ImageNet, the adversarial examples were so close to the original examples that the differences were indistinguishable to the human eye.
3. The same adversarial example is often misclassified by a variety of classifiers with different architectures or trained on different subsets of the training data.
4. Shallow softmax regression models are also vulnerable to adversarial examples.
5. Training on adversarial examples can regularize the model—however, this was not practical
at the time due to the need for expensive constrained optimization in the inner loop.
6. **Another great line I'm quoting directly from the paper:** "*These results suggest that classifiers based on modern machine learning techniques, even those that obtain excellent performance on the test set, are not learning the true underlying concepts that determine the correct output label. Instead, these algorithms have built a Potemkin village that works well on naturally occurring data, but is exposed as a fake when one visits points in space that do not have high probability in the data distribution. This is particularly disappointing because a popular approach in computer vision is to use convolutional network features as a space where Euclidean distance approximates perceptual distance. This resemblance is clearly flawed if images that have an immeasurably small perceptual distance correspond to completely different classes in the network’s representation.*"
7. Learnt about a [Potemkin village](https://en.wikipedia.org/wiki/Potemkin_village)

## The Linear Explanation of Adversarial Examples

1. Precision of an individual input feature is limited. For example, digital images discard all information below 1/255  of the dynamic range. For this reason, if the perturbation is small enough, we expect the classifier to return the same class for both input and slightly perturbed input.
2. Consider the dot product between the weight matrix and the adversarial example, it would just be the sum of the dot products between the weight matrix with the original input and the **weight matrix with the perturbation**. The part in bold is the increase and it gets bigger due to i) substituting 'w' with 'sign(w)' and ii) large input size.
3. This explanation shows that a simple linear model can have adversarial examples if its input has sufficient dimensionality. This hypothesis based on linearity is simpler, and can also explain why softmax regression is vulnerable to adversarial examples.

## Linear perturbation of Non-Linear models

1. **Neural networks are too linear to resist linear adversarial perturbation.**  LSTMs, ReLUs, and maxout networks are all intentionally designed to behave in very linear ways, so that they are easier to optimize. More nonlinear models such as sigmoid networks are carefully tuned to spend most of their time in the non-saturating, more linear regime for the same reason. This linear behavior suggests that cheap, analytical perturbations of a linear model should also damage neural networks.

![Panda + Nematode = Gibbon](https://cdn-images-1.medium.com/max/1200/0*k3QpRlbCJoiuUkNa)

2. **The Fast Gradient Sign Method**. It's great and all, but I've got no intention of typing LaTeX on GitHub Markdown all night.
3.  Other simple methods of generating adversarial examples are possible. For example, we also found that rotating 'x' by a small angle in the direction of the gradient reliably produces adversarial examples.

## Adversarial training of linear models versus weight decay

1. Consider logistic regression to gain intuition for how adversarial examples are generated in a simple setting. In this case, the fast gradient sign method is exact. Some stuff about L<sup>1</sup> decay was written, too drowsy to care.

## Adversarial training of deep networks

1. Reminder: The universal approximator theorem (Hornik et al., 1989) guarantees that a neural network with at least one hidden layer can represent any function to an arbitrary degree of accuracy so long as its hidden layer is permitted to have enough units. Two points of argument ensue naturally:
    * The universal approximator theorem does not say anything about whether a training algorithm will be able to discover a function with all of the desired properties.
    * Standard supervised training does not specify that the chosen function be resistant to adversarial examples. This must be encoded in the training procedure somehow.
2. **Cost function for adversarial training**
3. The adversarial training procedure can be seen as minimizing the worst case error when the data is perturbed by an adversary.
4.  Adversarial training can also be seen as a form of active learning, where the model is able to request labels on new points.
