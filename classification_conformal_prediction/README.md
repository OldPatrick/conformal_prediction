# conformal_prediction
1. Conformal Prediction: The Basics

Conformal prediction is a framework for constructing prediction sets (or prediction intervals in regression) that come with guaranteed coverage. Instead of providing a single point prediction (like "this image is a cat"), it provides a set of possible predictions ("this image could be a cat, dog, or bird"). The key is that this set is constructed in a way that ensures, with a specified probability (1 - α), the true label will be inside the prediction set.

Key components:

Nonconformity Score (NCS): This is a measure of how "strange" or "unlikely" a prediction is, given the training data. There are many ways to define a nonconformity score. A common choice for classification is 1 - p(y|x), where p(y|x) is the predicted probability of the true class y given the input x from your underlying machine learning model (e.g., a softmax output from a neural network). A high nonconformity score means the prediction is less confident.
Calibration Set: You split your data into a training set (to train the underlying model) and a calibration set. The calibration set is crucial for conformal prediction.
Calibration: You calculate the nonconformity scores for all the examples in the calibration set. This gives you a distribution of nonconformity scores.
Prediction: For a new, unseen data point, you calculate the nonconformity score for every possible label.
Prediction Set Construction: You form the prediction set by including all labels whose nonconformity scores are below a certain threshold. This threshold is determined by the α value and the distribution of nonconformity scores from the calibration set.
2. The Role of Alpha (α)

Alpha (α) is the significance level, and (1 - α) is the desired coverage level. It's the probability that you're willing to accept that the true label will not be in the prediction set. Crucially, α controls the size of the prediction sets.

High α (e.g., α = 0.2, meaning 80% coverage): You're willing to tolerate a higher error rate (20% of the time, the true label will be outside the set). This leads to smaller prediction sets. You're being more "picky" about which labels to include.
Low α (e.g., α = 0.01, meaning 99% coverage): You want to be very sure the true label is in the set (only a 1% chance of error). This leads to larger prediction sets. You're being less "picky" and including more labels.
3. Why Lower α Leads to Larger Prediction Sets and More Coverage

The key is how the threshold for inclusion in the prediction set is determined. This threshold is the (1 - α) quantile of the empirical distribution of nonconformity scores from the calibration set. Let's illustrate with an example:

Suppose your calibration set has 100 examples, and you've calculated their nonconformity scores. You sort these scores from smallest to largest.

α = 0.1 (90% coverage): You want the 90th percentile (1 - 0.1 = 0.9) of the nonconformity scores. This means you find the score at the 90th position in your sorted list. This score becomes your threshold. Any label for a new prediction with a nonconformity score below this threshold is included in the prediction set.
α = 0.01 (99% coverage): You want the 99th percentile (1 - 0.01 = 0.99) of the nonconformity scores. You find the score at the 99th position in your sorted list. This score is almost certainly higher than the 90th percentile score. Because the threshold is higher, more labels will have nonconformity scores below it, leading to a larger prediction set.
In simpler terms:

A lower α means you're demanding higher confidence.
Higher confidence requires a higher threshold on the nonconformity score.
A higher threshold means you include more labels in the prediction set to be sure you capture the true label.
More labels in the prediction set directly translates to higher coverage.
4. The Useless Model at 100% Coverage (α = 0)

If you set α = 0, you're aiming for 100% coverage. This means you want to guarantee the true label is always in the prediction set. The only way to achieve this is to include every possible label in the prediction set for every prediction.

Example: If you have a 10-class classification problem (classes 0-9), your prediction set for every input will be {0, 1, 2, 3, 4, 5, 6, 7, 8, 9}.
This is completely useless! You've achieved perfect coverage, but your prediction provides no information whatsoever. It's like saying, "I predict the outcome will be one of the possible outcomes." You haven't narrowed down the possibilities at all. The model has no discriminative power.

In summary:

Conformal prediction provides prediction sets with guaranteed coverage.
α controls the trade-off between coverage and the size (informativeness) of the prediction sets.
Lower α leads to higher coverage and larger, less informative prediction sets.
α = 0 (100% coverage) results in a trivial model that always includes all possible labels, providing no useful information. The goal is to find an α that balances coverage with informative (small) prediction sets.


The Guidance for conformal prediction with multi classes is, after revisiting the individual and group conformal methods:
If you care about conformal prediction, do not use score, use APS or RAPS with a randomized decision to take into account the last class or not