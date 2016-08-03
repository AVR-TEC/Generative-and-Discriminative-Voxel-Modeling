# Generative-and-Discriminative-Voxel-Modeling
Voxel-Based Variational Autoencoders, VAE GUI, and Convnets for Classification

![GUI](https://github.com/ajbrock/Generative-and-Discriminative-Voxel-Modelling/blob/master/doc/GUI3.png)

This repository contains code from the forthcoming paper, "Generative and Discriminative Voxel Modeling with Convolutional Neural Networks," and the [Voxel-Based Variational Autoencoders](https://www.youtube.com/watch?v=LtpU1yBStlU) and Voxel-Based Deep Networks for Classification videos.

## Installation
To run the VAE and GUI, you will need:

- [Theano](http://deeplearning.net/software/theano/) 
- [lasagne](http://lasagne.readthedocs.io/en/latest/user/installation.html)
- [path.py](https://github.com/jaraco/path.py)
- [VTK](http://www.vtk.org/) and its python bindings

If you want to plot latent space mappings, you will need [matplotlib](http://matplotlib.org/).

Download the repository and add the main folder to your PYTHONPATH, or uncomment and modify the sys.path.insert lines in whatever script you want to run.

## Preparing the data
I've included several .tar versions of Modelnet10, which can be used to train the VAE and run the GUI. If you wish to write more .tar files (say, of Modelnet40) for use with the VAE and GUI, download  [the dataset](http://modelnet.cs.princeton.edu/) and then see [voxnet](https://github.com/dimatura/voxnet).

For the Discriminative model, I've included a MATLAB script in utils to convert raw Modelnet .off files into MATLAB arrays, then a python script to convert the MATLAB arrays into either .npz files or hdf5 files (for use with [fuel](https://github.com/mila-udem/fuel)). 

The _nr.tar files contain the unaugmented Modelnet10 train and test sets, while the other tar files have 12 copies of each model, rotated evenly about the vertical axis. 

## Running the GUI
I've included a pre-trained model (VAE.npz) trained on Modelnet10, which can be used to run the GUI:

```sh
python Generative/GUI.py Generative/VAE.py datasets/shapenet10_test_nr.tar Generative/VAE.npz
```

## Training
If you wish to train a model, the VAE.py file contains the model configuration, and the train_VAE.py file contains the training code, which can be run like this:

```sh
python Generative/train_VAE.py Generative/VAE.py datasets/shapenet10_train.tar Generative/shapenet10_test.tar
```
By default, this code will save (and overwrite!) the weights to a .npz file with the same name as the config.py file (i.e. "VAE.py -> VAE.npz"), and will output a jsonl log of the training with metrics recorded after every chunk (a chunk being a set of minibatches loaded into shared memory). The binary reconstruction accuracy is evaluated on the test set after every N epochs (defined in the config file), and evaluates both false positives and false negatives.

A good model will obtain a very low false negative rate, while most any model can get near-perfect false positives (and therefore very high overall reconstruction accuracy).

## Notes
Discriminative models coming soon!

## Acknowledgments
This code was originally based on [voxnet](https://github.com/dimatura/voxnet) by D. Maturana.
