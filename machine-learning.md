![](/assets/ml-algorithm-cheatsheet.png)

## Naive Bayes Classifier
* "naive" conditional independence assumptions: each feature $$x_i$$ is conditionally independent of every other feature $$x_j$$.

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/416fe0a2f437394cc1bb3cbc79a6d3d21c9da43f)
* Based on the maximum a posteriori or MAP decision rule, Bayes classifier:

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/5ed52009429e5f3028302427a067822fdfc58059)

### Gaussian Naive Bayes
* the continuous values associated with each class are distributed according to a Gaussian distribution

![](https://wikimedia.org/api/rest_v1/media/math/render/svg/685339e22f57b18d804f2e0a9c507421da59e2ab)
* training: calculate $$\mu_k$$ and $$\sigma_k$$ from the data
* prediction: plug in features $$v_i$$ and calculate the probability of each class given those features then pick the class with the highest probability. 