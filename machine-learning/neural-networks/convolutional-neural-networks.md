

## Layer Output Size

* Conv Layer: `int((n_H - f + 2*p) / s) + 1, int((n_W - f + 2*p) / s)`
* Pool Layer: `int((n_H - f) / s) + 1, int((n_W - f + 2*p) / s)`

## Why Convolutions

Dramatically reduces training parameters by having following features:

* **Parameter sharing**: A feature detector \(a filter in CNN\) that's useful in one part of the image is probably useful in another part of the image.
* **Sparsity of connections**: In each layers, each output value depends only on a small number of inputs.
* robust to translation invariance to some degrees.



