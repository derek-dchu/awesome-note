## Orthogonalization

Specific tuning strategies that have independent effect

#### Chain of assumptions in ML

Usually a ML project proceeds when following assumptions are meeting one by one

1. Fit training set well on cost function: under-fitting -&gt; bigger network, Adam
2. Fit dev set well on cost function: over-fitting on training set -&gt; regularization, bigger training set
3. Fit test set well on cost function: over-fitting on dev set -&gt; bigger dev set
4. Performs well in real world: under-fitting real world data -&gt; change dev/test set or cost function

## Project Setup

#### Evaluation metric = 1 optimizing metric + N-1 satisficing metrics

Optimizing metric: one single number we want to optimize as the main evaluation metric to pick the "best" model to iterate on.

Satisficing metrics: filter out invalid models

#### Data Set Distribution

1. Choose a dev set and test dev to reflect data you expect to get in the future and consider important to do well on.
2. dev set and test dev should come from the same distribution.
3. For very large data set, we usually just need allocate small amount of data \(&lt;&lt; 20%\) for dev set and test set. Be sure Dev set is big enough to evaluate different models and test dev is big enough to give high confidence in the overall performance of the system.

##### Cleaning Up Incorrectly Labeled Data

1. Apply same process to both dev and test sets
2. Consider examining examples got right as well as one it got wrong
3. We don't need to correct train, as usually DL is robust for train and dev sets that come from slightly different distributions

##### Mismatched Training and Dev/Test Set

Sometimes, the real data is hard to obtain, so we obtain similar data from others sources and use them as training set. It is not recommender to combine real data with training set to generate data sets, because it will diluent the dev/test sets. Instead, we use a portion of real data as dev/test sets then put the remaining data into training set.

In this case, we have mismatched distribution of training and dev/test set and it is really hard to tell from comparing metrics between training set and dev set, whether the model has high bias or high variance.

To solve this problem, we can introduce **train-dev set** which is a small portion of training set. In other words, the gap train &lt;-&gt; train-dev reflects variance, and the gap train-dev &lt;-&gt; dev reflects mismatched data.

##### artificial data synthesis

To solve data mismatched problem.

## Comparing to Human-level Performance

Usually the accuracy of a model approach faster and faster to human-level performance then slow down and bound by the [Bayes Error Rate](https://en.wikipedia.org/wiki/Bayes_error_rate). Therefore, human-level error can be a proxy for Bayes Error.

Because when a model is worse than humans, we can:

* get labeled data from humans
* gain insight from manual error analysis: why did a person get this right?
* better analysis of bias/variance

#### Avoidable Bias

If the accuracy on training is very close to human-level, then we can assume that we have a very low bias. The gap between training error and human-level error is avoidable bias, because we can try to focus on reducing the gap first before reducing variance \(dev error\).

| Gap Between Errors | Meaning |
| :--- | :--- |
| human error &lt;-&gt; train | avoidable bias |
| train &lt;-&gt; train-dev | variance |
| train-dev &lt;-&gt; dev | mismatched data. Cannot tell which set is easier unless we compare them with its human-level error separately |
| dev &lt;-&gt; test | degree of overfitting to dev |

## Carry Out Error Analysis

Fail-fast iteration, it is good to first build a basic model and perform carry out error analysis at the beginning of ML project.

From basic model, we can learn that what errors it will make and what is order of them to be focused on.

Carry out error analysis: manually check for small portion of samples having perdition errors, then list individual errors with its % and comments.

| Sample | Mislabeled | Blurry | ... | Comments |
| :--- | :--- | :--- | :--- | :--- |
| 1 | X |  |  |  |
| 2 |  | X |  |  |
| ... |  |  |  |  |
| Total Error | 3% | 61% |  |  |

## Summary

* Set up dev/test set and metric
* Build initial system quickly, then iterate
* Use bias/variance analysis & error analysis to prioritize next steps

## Case Study

#### New metric come up

We were optimized metric A, and now metric B also has its requirement.

Rethink a metric M\(A, B\) which takes into account both A and B.

#### Generalize Overtime

New type of sample come up, how to generalize existing model for new type?

Use the new data to define a new evaluation metric \(using a new dev/test set\) taking into account the new type.

