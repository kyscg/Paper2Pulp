# [OverFeat: Integrated Recognition, Localization and Detection using Convolutional Networks](https://arxiv.org/abs/1312.6229)

## Introduction

1. Main point is to show that training a CNN to simultaneously classify, locate and detect objects in images boosts classification accuracy and the detection and localization accuracy of all tasks.
2. Localization and detection by accumulating predicted bounding boxes which avoids i) training on background samples and ii) bootstrapping training passes.
3. Not training on background lets the neural net focus on positive examples
4. First idea: Apply a ConvNet at multiple locations in the image, in a sliding window fashion over multiple scales. But here, windows might contain only certain parts of objects and not the full object (only the head of a dog but not the entire dog). This implies that there is decent classification but poor localization and detection.
5. Second idea: Produce a distribution over categories for each window. As well as produce a prediction of the location and size of the object. [For each window]
6. Third idea: Accumulate evidence for each category at each location and size.
7. The classification stuff is pretty straightforward.
8. In contrast to many sliding-approaches that compute the entire thing for every window throughout the image, we'll use the fact that convolutions share computations common to overlapping regions.
9. CNN models modified from [AlexNet](https://github.com/kyscg/Paper2Pulp/notes/AlexNet.md)[Fast and accurate].
10. Fine Stride Max Pooling added to the fifth layer of modified network.
11. There are three computer vision tasks here, in increasing order of difficulty, i) classification, ii) localization and iii) detection
12. In the classification part, each image is assigned a dingle label corresponding to the main object in the image.
    * Five guesses are allowed to find the correct answer (because image may contain other unlabeled objects)
13. Localization task is also similar (five guesses) but in addition, a bounding box for the predicted object must be returned.
    * To be considered correct, prediction must match ground truth by at least 50% and be labelled with the correct class.
14. Detection task differs from localization [multiple number of objects (including 0) and false positives penalized by mAP measure]
15. [Digression: mean Average Precision for Object Detection](https://medium.com/@jonathan_hui/map-mean-average-precision-for-object-detection-45c121a31173)
16. Model Design: Layers 1 to 5 are similar to AlexNet with the following differences:
    * No contrast normalization used.
    * Pooling regions are non-overlapping.
    * Larger first and second layer feature maps thanks to a smaller stride (2 instead of 4).
17. Multiscale classification: 
    * The problem with multi-view voting: ignores many regions on the image - computationally redundant when views overlap - only applied at single scale.
    * Instead, densely run network at each location and at multiple scales. The sliding window approach is inherently efficient in the case of ConvNets.
    * Six different scales of input result in six Layer5 un-pooled maps of different spatial resolution.
    * Each of these undergoes max-pooling (3 by 3) for pixel offset combinations of delta_x, delta_y = {0, 1, 2}
    * Classifier (Layer 6, 7, 8) has fixed input size of (5 by 5) and produces a C-dimensional output.
    * Classifer applied in sliding window fashion, giving C-dimensional output maps.
    * Output maps for different delta_x, delta_y combinations are reshaped into a single 3D output map.
18. Note: At test time, these layers are effectively replaced by convolution operations with kernels of (1 by 1) spatial extent.
19. Localization: Start from classification trained network, replace classifier layers by a regression network and train it to predict boxes. We then combine the two. 
20. Non-Max Suppression Algorithm: 
    * Each output prediction is [pc, bx, by, bh, bw]
    * Discard all boxes with pc <= 0.6
        * While there are any remaining boxes
            * Pick box with largest pc and output as prediction.
            * Discard remaining boxes with IoU >= 0.5 with output box of previous step.
21. For multiple classes, perform the above independently on each class.
