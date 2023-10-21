---
title: Building a simple Rust Neural Network from scratch
date: 2023-10-21 14:10:00 0300
categories: [AI, Neural Networks]
tags: [ai, neuralnetworks]     # TAG names should always be lowercase
---

# Goal

I've recently took upon myself the challenge of building a Neural Network to identify digits (i.e.: from 0 to 9) using Rust for study purposes. However, I did not want to use existing libraries, but rather build it from scratch so that I could learn the concepts behind Neural Networks. 

## Dataset

I've decided to use the MNIST dataset available in [Kaggle](https://www.kaggle.com/datasets/oddrationale/mnist-in-csv), which is a dataset containing thousands of digits data as images of 28x28 pixels. There are two files, one used for training and another one used for testing the Neural Network.

## Layers

The Neural Network that I've created contains three layers. 

The input layer has a size of 784 (28px * 28px), which will receive the pixel data of each image. 

```rust
let mut model = DeepNeuralNetwork {
        layers: vec![
            Layer {
                size: 784,
                activation_function: Box::new(ReLU {}),
            },
            Layer {
                size: 10,
                activation_function: Box::new(ReLU {}),
            },
            Layer {
                size: 10,
                activation_function: Box::new(Sigmoid {}), // TODO: Not ideal for outputs w/ multiple classes, but my softmax is broken :(
            },
        ],
        learning_rate: 0.1,
        params: HashMap::new(),
    };
```

Then there is an hidden layer with size 10, and finally an output layer of size 10 as well. 

The output layer has to contain 10 classes (size 10), the reason for that is because this is not a neural network that provides a binary output or true or false (e.g.: a neural network that checks if an input image contains a cat), but rather a network that is performing a multiclass classification. In other words, we are giving it an input that can be anything from zero to nine, and we are asking the neural network to classify. In this case, assuming we give an input which is the pixels of an image containing the number 9, the output layer will be similar to:

| output | probability |
|--------|-------------|
| 0      | 0.0417      |
| 1      | 0.0178      |
| 2      | 0.0946      |
| 3      | 0.1927      |
| 4      | 0.1958      |
| 5      | 0.0125      |
| 6      | 0.2850      |
| 7      | 0.1818      |
| 8      | 0.3102      |
| 9      | 0.9127      |

As you can see, it outputs the likelihood of the input pixels to be classified as each of the possible alternatives, nine being the higher in our case.

Other types of neural networks are:

- Multilabel classification 
- Regression 
- Sequence to sequence
- Generative models
- ...p

## Activation functions

I've also wanted to code the activation functions manually instead of relying on existing implementation. In the network I've developed, I'm using ReLU for the hidden layer and Sigmoid for the output layer. 

Sigmoid is not the best solution for classification networks however and Softmax should in theory work better but I did not implement my version of Softmax yet. 

## Code 

You can check the full code in my [rust-neuralnets repo](https://github.com/gabrielricci/rust-neuralnets/tree/main). Please feel free to share any feedbacks.
