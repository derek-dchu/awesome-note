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

## Comparing to Human-level Performance

Usually the accuracy of a model approach faster and faster to human-level performance then slow down and bound by the [Bayes Error Rate](https://en.wikipedia.org/wiki/Bayes_error_rate). Therefore, human-level error can be a proxy for Bayes Error.

Because when a model is worse than humans, we can:

* get labeled data from humans
* gain insight from manual error analysis: why did a person get this right?
* better analysis of bias/variance

#### Avoidable Bias

If the accuracy on training is very close to human-level, then we can assume that we have a very low bias. The gap between training error and human-level error is avoidable bias, because we can try to focus on reducing the gap first before reducing variance \(dev error\). 

## Case Study

#### New metric come up

We were optimized metric A, and now metric B also has its requirement.

Rethink a metric M\(A, B\) which takes into account both A and B.

#### Generalize Overtime

New type of sample come up, how to generalize existing model for new type?

Use the new data to define a new evaluation metric \(using a new dev/test set\) taking into account the new type.



