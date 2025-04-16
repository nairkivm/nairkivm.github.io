---
layout: page
title: "#015 : Cat and Dog Images Classification"
cover-img: /assets/img/project-014/illustration.png
---

> #### Key tools/skills:
> - Nural networks (Scikit learn, Tensorflow)
> - Tensorflow Serving (deploy the model)

> Project : [_https://github.com/nairkivm/cat-dog-classifiers_](https://github.com/nairkivm/cat-dog-classifiers)

In this project, I built a simple yet powerful image classification model that can do exactly that! I used TensorFlow and Keras with a convolutional neural network (CNN) and trained it using a dataset from Kaggle.

## Dataset:
[Dog and Cat Classification Dataset – Kaggle](https://www.kaggle.com/datasets/bhavikjikadara/dog-and-cat-classification-dataset)
This dataset contains 12,000+ images of cats and 12,000+ images of dogs.

## Step-by-Step Workflow

1. Data Preparation

I started by downloading and extracting the dataset, which contained images of dogs and cats. I wrote a small script to:
 - Preview sample images
 - Organize them into folders (/Dog, /Cat)
 - Count them (around 12,000 images per category initially)

<figure style="text-align: center;">
    <img src="/assets/img/project-015/preview-cat-dog.png" alt="Preview of the datasets." style="width:100%;">
    <figcaption style="font-size: 0.8em;">Preview of the datasets.</figcaption>
</figure>

1. Data Augmentation

To improve generalization and prevent overfitting, I applied image augmentation using Keras’ ImageDataGenerator:

- Rotation
- Warp shifting
- Zooming, etc.

This expanded the dataset to ~14,000 images per category, improving the diversity of training examples.

3. Data Splitting

I split the dataset using an 80:20 ratio:
- 80% for training (with 20% of this used for validation)
- 20% for testing

4. Model Building

I built the model using a Sequential CNN architecture. Here's a brief overview of important layers:

- Conv2D: Extracts spatial features from images.
- MaxPooling2D: Reduces spatial dimensions, preserving key features.
- BatchNormalization: Normalizes activations for faster, more stable training.
- Dropout: Prevents overfitting by randomly dropping units during training.
- Dense: Fully connected layer for final classification.

The model was compiled with an appropriate loss function (binary_crossentropy) and optimizer (Adam).

5. Training & Evaluation

I trained the model with callbacks like:
- EarlyStopping – to halt training when no improvement
- ReduceLROnPlateau – to lower learning rate when accuracy plateaus

## Performance Results:

Training Accuracy: 95.55%

Testing Accuracy: 86.00%

<figure style="text-align: center;">
    <img src="/assets/img/project-015/accuracy-vs-lost.png" alt="Accuracy and lost evaluation over the training epoch." style="width:100%;">
    <figcaption style="font-size: 0.8em;">Accuracy and lost evaluation over the training epoch.</figcaption>
</figure>

<figure style="text-align: center;">
    <img src="/assets/img/project-015/confusion-matrix.png" alt="Confusion matrix of the result." style="width:100%;">
    <figcaption style="font-size: 0.8em;">Confusion matrix of the result.</figcaption>
</figure>

## Saving the Model

I saved the trained model in three formats for broader deployment possibilities:

- .h5 (standard Keras format)
- .tflite (TensorFlow Lite)
- tfjs (TensorFlow.js)

## Deployment with TensorFlow Serving

I tested the model using TensorFlow Serving in a Docker container. The deployment steps included:

- Loading the model into a mounted container
- Running the TensorFlow model server
- Sending test images via POST request to the REST API

### Test Results

I tried predicting two images:
- A real photo of my cat 🐱
- A meme image of Doge 🐶

<figure style="text-align: center;">
    <img src="/assets/img/project-015/test-cat-dog.png" alt="The model is correctly predicting the two images." style="width:100%;">
    <figcaption style="font-size: 0.8em;">The model is correctly predicting the two images.</figcaption>
</figure>

The model successfully classified both images correctly, even though these were not part of training/testing data.

## Summary

- Feature	Status
- 14k+ images/class	✅
- CNN with BatchNorm & Dropout	✅
- Acc > 85% on test	✅
- Saved in h5, tflite, tfjs	✅
- Real-world inference	✅

> Posted on 2025-04-16