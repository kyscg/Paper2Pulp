# [Intriguing Properties of Neural Networks](https://arxiv.org/abs/1312.6199)

## Introduction

1. The key findings are the following:
   - The space, rather than the individual units, that contains the semantic information in the high layers of neural networks.
   - We can cause the network to misclassify an image by applying a certain hardly perceptible perturbation, which is found by maximizing the network’s prediction error.
   - The same perturbation can cause a different network, that was trained on a different subset of the dataset, to misclassify the same input. Also termed **transferability** in CS230 Stanford.
2. This review will mostly deal with the adversarial examples part because I said so.

## Adversarial Instances Are General To Different Models and Datasets

1. They posit that advertorials exploiting a particular network architectures are also hard to classify for the others. They illustrate it by creating adversarial instances yielding 100% error-rate on the target network architecture and using these on the another network. It is shown that these adversarial instances are still hard for the other network (a network with 2% error-rate degraded to 5%). Of course the influence is not that strong compared to the target architecture (which has 100% error-rate).

## Adversarial instances are more significant to higher layers of networks

1. As you go to higher layers of the network, instability induced by adversarial instances increases as they measure by Lipschitz constant. This is justifiable observation with that the higher layers capture more abstract semantics and therefore any perturbation on an input might override the constituted semantic. (For instance a concept of "dog head" might be perturbed to something random).

## Auto-Encoders are more resilient to adversarial instances

1. AE is an unsupervised algorithm and it is different from the other models used in the paper since it learns the implicit distribution of the training data instead of mere discriminant features. Thus, it is expected to be more tolerant to adversarial instances. It is understood by Table2 that AE model needs stronger perturbations to achieve 100% classification error with generated adversarial examples.

## Thoughts

1. One intriguing observation is that shallow models with no hidden units are yet to be more robust to adversarial instances created from the deeper models. It questions the claim of generalization of adversarial instances. I believe, if the term generality is supposed to be hold, then a higher degree of susceptibility ought to be obtained in this example (and in other too).

2. I am also happy to see that the unsupervised method is more robust to adversarial as expected since I believe the notion of general AI is only possible with the unsupervised learning which learns the space of data instead of memorizing things. This is also what I plan to examine after this paper to see how the new tools like Variational Auto Encoders behave against adversarial instances.

3. I believe that it is really hard to fight with adversarial instances, especially the ones created by counter optimization against a particular supervised model. A supervised model always has flaws to be exploited in this manner since it memorizes things and when you go beyond its scope (especially with adversarial instances which are of low probability), it makes natural mistakes. Besides, it is known that a neural network converges to local minimum due to its non-convex nature. Therefore, by definition, it has such weaknesses.

4. After having read the paper: I either haven't understood the paper correctly, or it is mainly hogwash.

   - It's conclusion reads: "_The adversarial negatives appears to be in contradiction with the network’s ability to achieve high generalization performance. Indeed, if the network can generalize well, how can it be confused by these adversarial negatives, which are indistinguishable from the regular examples?_"

   - So they set up an optimization problem that finds images that are as indistinguishable as possible from real data points but are wrongly classified, and they end up with -- drumrolls please -- images that are indistinguishable from real data points but are wrongly classified! Intriguing indeed, L-BFGS works as advised, alert the press?

   - I would be more surprised if such examples wouldn't exist. Just look at any papers that show any first-layer filters on MNIST (and I've never read a deep learning paper that doesn't include such pictures) --- it isn't hard to imagine that it's easy to confound any of these edge/stroke detectors with a few cleverly modified pixels (at least that's the impression I get visually). Especially if you look at all of them at the same time to determine which pixels to modify to turn some detectors off and others on -- and the optimization task is of course able to do this. The really clever idea behind this paper -- and this should've been the main point of the experiment in my eyes, as it's the really interesting bit -- is using these new distortions to train the classifiers and see if it improves generalization. Yet this part is missing from the analysis, and it really makes me wonder why that is.

   - What I also don't get is their argument against disentangling. If I understood correctly they found out that images that activate a bunch of units the same way (i.e., "in a random direction") look similar. How does that contradict disentangling?

   - After thinking about it some more: so their main surprising result is actually that maybe the classification space isn't as smooth as usually thought/claimed? My multi-dimensional understanding isn't the best, but isn't that also somehow connected to the curse of dimensionality or the manifold assumption (i.e., when dimensions are so high, the subspace/manifold your data points lie on could be "folded" so weirdly that only taking a small step in the "right" direction lands you somewhere else entirely)?

5. For a paper that wants to find out the distinctive weaknesses of the class of functions that neural networks learn, they sure don't spend much time comparing against a control (like SVMs, Random Forests, etc). How do they know these 'blind spots' are a weakness of neural networks and not inherent in the dataset and specifically when the only information available is object category.

6. Don't get me wrong, I agree with the spirit of this paper but I'm not convinced that these properties are specific to neural networks and not the information available in the training set/task. This paper doesn't provide any attempt to disentangle the two.

7. Also, they should evaluate the likelihood of the 'adversarial' examples they generate. Visual inspection isn't going to cut it. Furthermore, this isn't surprising to find small perturbations that can change object category in high dimensional spaces like images. The 'density' of these adversarial examples should increase exponentially as dimensionality increases.

8. I'm also surprised they didn't try contractive autoencoders (or didn't mention it if they did). The cost function of CAEs should help address the issue of local regions of the manifold having low density. They practically re-derive the contractive penalty in section 4.3.

9. Nevertheless, I like these sort of retrospective papers reviewing what neural networks are doing.
