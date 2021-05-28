# [Generative Adversarial Networks](https://arxiv.org/abs/1406.2661)

## Introduction

1. We simultaneously train two models: a generative model, G, that captures the data distribution, and a discriminative model, D, that estimates the probability that a sample came from the training data rather than G.
2. The way we train G is by ensuring that we maximize the possibility of D making a mistake.
3. Deep _generative_ models have not been so successful, mostly because of the difficulty of approximating loads of specific functions. The method proposed by the authors efficiently steps over these intricacies.
4. The while thing looks like a game, the G network mainly functions to 'generate' fake data so that D gets fooled while D improves it's detection framework every iteration.

![The G/D game](https://skymind.ai/images/wiki/GANs.png)

## Adversarial Nets

1. G is the generated mapping from the input with some learned parameters. D is a second network that outputs a scalar representing the probability that it's input came from real world data rather than from G.
2. We train D to maximize the probability of assigning the correct label to both training examples and samples from G. We simultaneously train G to minimize log(1 − D(G(z)))
3. **IMPORTANT COST FUNCTION**, when I say important, I mean it is essential to understand how all the words above turned into math.
4. I feel like at this point, the reader must be thinking of ways to efficiently implement this and spoiler, figure out what's wrong with the cost function.
5. A trick that I think would work here:
   - D is easier to train than G, but **only** as D improves, G can improve.
   - The performance of D can be seen as an upper-bound to what G can achieve.
   - We alternate between k steps of optimizing D and one step of optimizing G. This results in D being maintained near its optimal solution, so long as G changes slowly enough.
   - The authors also mention that optimizing D to completion in the inner loop of training is computationally prohibitive,and on finite datasets would result in overfitting.
6. And remember I said something was wrong with the cost function? Figure it out and derive the correct one, it involves pretty elementary math (and some imagination).

## Theoretical Results

1. Check out Figure 1, Equation 1 and Algorithm 1 from the paper.
2. G implicitly defines a probability distribution p<sub>g</sub>. We would like an algorithm to converge to a good estimate of p<sub>data</sub>.
3. The algorithm acts theoretically - non-parametric setting - model has infinite capacity.
4. We'll show that the G/D game has global optimum at p<sub>g</sub> = p<sub>data</sub>. We'll also see how the algorithm optimizes the cost function ie. Equation 1 in the paper.
5. Algorithm:

   - Minibatch Stochastic Gradient Descent Training of GANs
   - Number of steps to apply to discriminator, k, is a hyperparameter.

   ```pseudo
   for <no. of training iterations> do:
       for k steps do:
           1. Sample minibatch of m noise samples {z_1, ..., z_m} from noise prior.
           2. Sample minibatch of m samples {z_1, ..., z_m} from data generating distribution.
           3. Update discriminator by ascending its stochastic gradient
       end for
       1. Sample minibatch of m noise samples {z_1, ..., z_m} from noise prior.
       2. Update generator by descending its stochastic gradient.
   end for
   ```
