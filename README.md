# NLP Quick Start
## [See Notebook for Code and Walkthrough](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb)

There are a wealth of fancy NLP algorithms available today - particularly using transformers, which has overshadowed a lot of the basics of NLP such as clustering and classification. However, simple algorithms are much easier to scale and often provide an excellent basis before building more complicated models.

I'm going to work through the Twitter Disasters dataset originally made available by [Crowdflower](https://appen.com/resources/datasets/). I found it currently available [here](https://data.world/crowdflower/disasters-on-social-media).

# Abstract

In this notebook, I use a simple logistic regression classifier on a dataset of 10,000 tweets to predict whether the tweet refers to a true "disaster" event, or whether the tweet is irrelevant. I focus on the interpretability of simple classification models and what that means for text data. I look at a few methods of creating text embeddings for NLP tasks and explain the use and demonstrate the significant advantages of incorporating semantic meaning into NLP tasks using models such as Word2Vec.

I extend on the interpretability of NLP models by using LIME to understand predictions made on Word2Vec embeddings, and finally, I attempt to incorporate the syntactic structure of tweets into a model's predictions by building a 1D CNN for text classification on top of Word2Vec embeddings. 

The point of this notebook is to serve as an intro to NLP to get direction for where and how to proceed in improving the performance of text clustering and classification algorithms, whether that entails further dataset processing - a commonly productive endeavour - or in employing a more complicated model.

The steps I've taken here constitute a good baseline to get started on an NLP project, though by no means are they comprehensive. 

# Table of Contents:
- [Intro](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Intro)
- [The Dataset](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#The-Dataset)
  - [Inspecting and Cleansing the Dataset](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Inspecting-and-Cleansing-the-Dataset)
  - [Tokenization](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Tokenization)
- [Text Embeddings](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Text-Embeddings)
  - [Bag of Words](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Bag-of-Words)
  - [Visualizing the Embeddings with LSA](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Visualizing-the-Embeddings-with-LSA)
- [Fitting a Classifier for Baseline Performance](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Fitting-a-Classifier-for-Baseline-Performance)
  - [Evaluation](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Evaluation)
  - [Inspection](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Inspection)
  - [Interpretability](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Interpretability)
- [TF-IDF Bag of Words](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#TF-IDF-Bag-of-Words)
- [Understanding Semantic Meaning](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Understanding-Semantic-Meaning)
  - [Word2Vec](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Word2Vec)
- [Intepretability with LIME](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Intepretability-with-LIME)
- [Using CNNs for Text Classification](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Using-CNNs-for-Text-Classification)
  - [Preprocessing - Padding and One-Hot Encoding](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Preprocessing:-Padding-and-One-Hot-Encoding)
  - [1D Word2Vec CNN](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#1D-Word2Vec-CNN)
- [Take-Away](https://nbviewer.jupyter.org/github/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/NLP_basics_keras_nltk_lime.ipynb#Take-Away)

## Cite
If you find this work useful in your own research, please cite as follows:

```
@misc{Zenkov-NLP-basics,
  author = {Zenkov, Ilia},
  title = {NLP-basics-keras-nltk-lime},
  year = {2020},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/IliaZenkov/NLP-basics-keras-nltk-lime}},
}
```

## Licence

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://github.com/IliaZenkov/NLP-basics-keras-nltk-lime/blob/main/LICENSE)
