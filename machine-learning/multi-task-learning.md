## Transfer Learning

#### When To Use

1. Task A and B have similar input
2. Task A has a lot more data than task B
3. Low level features from A could be helpful for learning B

## Multi-task Learning

We stack multi-labels \(from different tasks\) into one label vector and define a cost function which sums over costs of different labels.

Note: non-fully labeled sample can also be used by disregarding unknown label of individual task.

#### When To Use

1. Training on a set of tasks that could benefit from having **shared lower-level features \(resulting in one network\).**
2. Usually: Amount of data we have for each task is quite similar. So we can combine them to train one network.
3. A big enough network can do well on all the tasks.

## End-to-end Learning

Trained a network to map input to output directly without the need of intermediate hand-designed components \(feature engineering\)

#### When To Use

1. Large amount of end-to-end data



