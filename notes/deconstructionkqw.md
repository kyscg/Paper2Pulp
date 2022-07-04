# [On the Importance of Deconstruction in Machine Learning Research](https://www.youtube.com/watch?v=kY2NHSKBi10)

Talk by Dr. Kilian Q. Weinberger at the NeurIPS Retrospective Workshop, December 2020. [[Slides](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbkp5ajFDNmZxd29kNWQzQ0E1bGJsWktTLWNwd3xBQ3Jtc0tua1p0bTU5OWZudm9DNlJZOW5lSkp6Y0M2MG13R0dZbktEajZtbXlnSGJ2czdJaEIwTXBMVEtmSXNvd1lYMlNSc3dfaklhWld2Mkl6TmpXYUlaNm1CcGsyMkFsT1pIbGRTbEZ2d0JtSW03cDhkbUlrUQ&q=https%3A%2F%2Fslideslive.com%2F38938218%2Fthe-importance-of-deconstruction%3Ffbclid%3DIwAR2zQ0y6WFmxv-2sHPVOhd8UirYW2vyU7hlSnnh4rXX3NtOma_kR9aG10Y4&v=kY2NHSKBi10)]

---

## [Simple Black-box Adversarial Attacks](https://arxiv.org/abs/1905.07121), ICML 2019

- [Intriguing properties of neural networks](https://arxiv.org/abs/1312.6199), small perturbations changes to input changes model output.

- Track changes to input image while it turns into an adversarial attack (extremely difficult search problem)

- It's easy if it's a white-box attack (just find the gradients), but it's hard if it's a black-box attack, and we try to approximate gradients by very costly queries.

- Instead of approximating gradients, we try to approximate the black box itself, using Gaussian processes.

- Random coordinate descent, change values of pixels in the image until it misclassifies. And more importantly, keep changes that drive the model towards misclassification. (extremely easy search problem)

- The reason this works is that this is a one-to-many problem, and many random peturbations work to make the model misclassify. The right solution told us something about the nature of the problem.

## [Pseudo-LiDAR from Visual Depth Estimation: Bridging the Gap in 3D Object Detection for Autonomous Driving](https://arxiv.org/abs/1812.07179), CVPR 2019

- Lidar pipeline: Frontal images + LiDAR point cloud through PointRCNN gives bounding boxes prediction.

- Stereo pipeline: Stereo images through disparity estimation gives disparity maps. Then we take the stereo images + depth information and get 3D bounding boxes.

- From depth maps to bird-eye views because convolving depth maps blurs the depths.

## [SimpleShot: Revisiting Nearest-Neighbor Classification for Few-Shot Learning](https://arxiv.org/abs/1911.04623),

- Train on some pictures, and test on completely new classes.

- KNN is a strong baseline for few-shot learning. After the following preprocessing steps
    - Subtract mean
    - Normalize features
    - Use cosine similarity

- Representation matters a lot

## Other papers

- [Simplifying Graph Convolutional Networks](https://arxiv.org/abs/1902.07153), ICML 2019

- [On Calibration of Modern Neural Networks](https://arxiv.org/abs/1706.04599), ICML 2017

## Conclusion

- Deconstruct, strive for simplicity

- Publish a **new understanding** of the problem.

---

