* t-SNE: A non-linear dimensionality reduction technique
* Word embedding: When learning word embeddings, we create an artificial task of estimating P\(target∣context\). It is okay if we do poorly on this artificial prediction task; the more important by-product of this task is that we learn a useful set of word embeddings.
* word2vec: In the word2vec algorithm, you estimate P\(t∣c\), c and t are chosen to be nearby words. θt and ec are both trained with an optimization algorithm such as Adam or gradient descent.
* GloVe: θi and ej should be initialized randomly at the beginning of training. Xij is the number of times word i appears in the context of word j. The weighting function f\(.\) must satisfy f\(0\)=0.
* Beam Search: Increase beam width won't converge after fewer steps. We have to use sentence normalization, otherwise, the algorithm will tend to output overly short translations.
* attention model: α&lt;t,t′&gt; is generally larger for values of a&lt;t′&gt; that are highly relevant to the value the network should output for y&lt;t&gt;. ∑t′α&lt;t,t′&gt;=1. The network learns where to “pay attention” by learning the values e&lt;t,t′&gt;, which are computed using a small neural network. We can't replace s&lt;t−1&gt; with s&lt;t&gt; as an input to this neural network. This is because s&lt;t&gt; depends on α&lt;t,t′&gt; which in turn depends on e&lt;t,t′&gt;; so at the time we need to evaluate this network, we haven’t computed s&lt;t&gt; yet.



