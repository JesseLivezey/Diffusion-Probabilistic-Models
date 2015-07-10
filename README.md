# Diffusion Probabilistic Models

This repository provides a reference implementation of the method described in the paper:<br>
> Deep Unsupervised Learning using Nonequilibrium Thermodynamics.<br>
> Sohl-Dickstein, Jascha and Weiss, Eric A. and Maheswaranathan, Niru and Ganguli, Surya.<br>
> International Conference on Machine Learning. 2015<br>
> http://arxiv.org/abs/1503.03585

This implementation builds a generative model of data by training a Gaussian diffusion process to transform a noise distribution into a data distribution in a fixed number of time steps.
The mean and covariance of the diffusion process are parameterized using deep supervised learning.
The resulting model is tractable to train,
easy to exactly sample from,
allows the probability of datapoints to be cheaply evaluated,
and allows straightforward computation of conditional and posterior distributions.

## Using the Software

In order to train a diffusion probabilistic model on the default dataset of MNIST, simply install dependencies (see below), and then run
``python train.py``.

### Dependencies

1. Install `Blocks` and its dependencies following [these instructions](http://blocks.readthedocs.org/en/latest/setup.html)
2. Setup `Fuel` and download MNIST following [these instructions](https://github.com/mila-udem/fuel/blob/master/docs/built_in_datasets.rst).

### Output

The objective function being minimized is the bound on the negative log likelihood in bits per pixel, minus the negative log likelihood under an identity-covariance Gaussian model. That is, it is the negative of the number in the rightmost column in Table 1 in the paper.

Logging information is printed to the console once per training epoch, including the current value of the objective on the training set.

Figures showing samples from the model, parameters, gradients, and training progress are also output periodically (every 25 epochs by default -- see ``train.py``).

The samples from the model are of three types -- standard samples, samples inpainting the left half of masked images, and samples denoising images with Gaussian noise added (by default, the signal-to-noise ratio is 1). This demonstrate the straightforward way in which inpainting, denoising, and sampling from a posterior in general can be performed using this framework.

Here are samples generated by this code after 825 training epochs on MNIST, trained using the command `run train.py`:<br>
<img src="https://github.com/Sohl-Dickstein/Diffusion-Probabilistic-Models/blob/master/samples-_t0000_epoch0825.png" width="500">

Here are samples generated by this code after 1700 training epochs on CIFAR-10, trained using the command `run train.py --batch-size 200 --dataset CIFAR10 --plot_before_training True --model-args "n_hidden_dense_lower=1000,n_hidden_dense_lower_output=5,n_hidden_conv=100,n_layers_conv=6,n_layers_dense_lower=6,n_layers_dense_upper=4,n_hidden_dense_upper=100"`:<br>
<img src="https://github.com/Sohl-Dickstein/Diffusion-Probabilistic-Models/blob/master/samples-_t0000_epoch1700.png" width="500">


## Miscellaneous

**Multi-scale convolution** - For the examples in the paper, we used multi-scale convolutional layers when applying the algorithm to large images. These are not yet incorporated into this implementation.
I am in the process of implementing multi-scale convolutional layers in Blocks, so this should be available soon.

**Different nonlinearities** - In the paper, we used soft ReLU units in the convolutional layers, and tanh units in the dense layers.
In this implementation, I use leaky ReLU units everywhere.

**Original source code** - This repository is a thorough refactoring of the code used to run the experiments in the published paper.
In the spirit of reproducibility, if you email me a request I am willing to share the original source code.
It is poorly commented and held together with duct tape though.
For most applications, you will be better off using the reference implementation provided here.

**Contact** - I would love to hear from you. Let me know what goes right/wrong! <jascha@stanford.edu>
