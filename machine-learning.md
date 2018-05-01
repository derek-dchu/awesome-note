![](/assets/ml-algorithm-cheatsheet.png)

## Naive Bayes Classifier

* "naive" conditional independence assumptions: each feature $$x_i$$ is conditionally independent of every other feature $$x_j$$.


$$
p(C_k|x_1,...,x_n)=\frac{1}{Z}p(C_k)\prod_{n}^{i=1}p(x_i|C_k)
$$


* Based on the maximum a posteriori or MAP decision rule, Bayes classifier:


$$
\widehat{y} = \mathop{\operatorname{argmax}}_{k\in \{1,...,K\}}p(C_k)\prod_{i=1}^{n}p(x_i|C_k)
$$


### Gaussian Naive Bayes

* the continuous values associated with each class are distributed according to a Gaussian distribution


$$
p(x = v|C_k) = \frac{1}{\sqrt{2\pi \sigma_{k}^{2}}} e^{-\frac{(v-\mu_k)^{2}}{2\sigma_{k}^{2}}}
$$


* training: calculate $$\mu_k$$ and $$\sigma_k$$ from the data
* prediction: plug in features $$v_i$$ and calculate the probability of each class given those features then pick the class with the highest probability. 



