# [You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/abs/1506.02640)

## Introduction

1. A single ConvNet simultaneously predicts multiple bounding boxes and class probabilities for those boxes.
2. The system divides the input image into an (5 by 5) grid. If the center of an object falls into a grid cell, that grid cell is responsible for detecting that object.
3. Each grid cell predicts B bounding boxes and confidence scored for each box. Confidence is defined as Pr(object) * IoU_truth_pred.
    * If no object exists, confidence should be 0 and if there, confidence should be IoU.
4. Each box returns; x, y, w, h, confidence where (x, y) represent center of box relative to grid. (w, h) are predicted relative to whole image. Confidence is as above.
5. Each grid cell also predicts 'C' conditional class probabilities Pr(Class_i | Object). We only predict one set of class probabilities per grid cell, regardless of number of boxes B.
6. At test time, we multiply conditional class probabilities and individual box confidence predictions.
    * Pr(Class_i | Object) * Pr(Object) * IoU_truth_pred = Pr(Class_i) * IoU_truth_pred
7. This gives us class-specific confidence scores for each box. These scored encode both the probability of that class appearing in the box and how well the predicted box fits the object.
8. System models detection as a regression problem. Divides system into (5 by 5) grid and each grid cell predicts B boxes, confidence for them and C conditional probabilities. Predictions encoded as (5 by 5 by (B*5 + C)) tensor.
9. Architecture: 24 Conv layers followed by 2 F.C layers. Alternating (1 by 1) Conv layers reduce feature space from preceding layers.
10. Training:
    * Used linear activation for last layer and leaky ReLU for others.
    * Optimized for sum-squared error due to the ease even if it doesn't maximize AP. It weights localization error equally with classification error.
    * Also, many grid cells don't contain images implying that confidence scores near zero overpowers gradients. This leads to model instability and causes it to diverge early.
    * To remedy this, increase loss from bounding box coordinate predictions and decrease loss from confidence predictions for boxes that don't have objects. We use lambda_coord = 5 and lambda_noobj = 0.5 for this.
    * Sum-squared error equally weights errors in large and small boxes. Our error metric should reflect that small deviations in large boxes matter less than in small boxes. To address this in part, we predict square root on bounding box width and height.
11. **HUGE LOSS FUNCTION**
12. Limitations of YOLO:
    * YOLO imposes string spatial constraints.
    * Struggles to generalize to new or unusual objects.
    * Sum-squared error screws things up slightly.
