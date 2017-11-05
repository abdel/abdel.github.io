---
published: false
title: 'IMDb Sentiment Analysis with Keras'
date: '2017-10-09'
excerpt: 
layout: post
---

I was first introduced to Keras through the fast.ai Part 1 course, and I really enjoyed using it. I was interested in exploring it further by utilising it in a personal project. I had an opportunity to do this through a university project where we are able to research a machine learning topic of our choice. I decided to explore and build a model for sentiment analyis on movie reviews using the [Large Movie Dataset](http://ai.stanford.edu/~amaas/data/sentiment/). I also took this a bit further, and worked on deploying the Keras model alongside a web application.

### Models
I experimented with different model architectures: Recurrent neural network (RNN), Convolutional neural network (CNN) and Recurrent convolutional neural network (RCNN). The RCNN architecture was based on the [paper](http://www.nlpr.ia.ac.cn/cip/~liukang/liukangPageFile/Recurrent%20Convolutional%20Neural%20Networks%20for%20Text%20Classification.pdf) by Lai et al. in which they aim to combine the benefits of both architectures, where the CNN can capture the semantics of the text, and the RNN can handle contextual information.

The Jupyter notebooks for each model can be found

### Deploying the Keras Model


### Resources
- [Source Code](https://github.com/abdel/imdb-sentiment-analysis)
- [Project Report](https://i.abdel.me/IqkrM6rMNU/Sentiment-Analysis-on-Movie-Reviews.pdf)

### Credits

- [Deploying your Keras model](https://medium.com/@burgalon/deploying-your-keras-model-35648f9dc5fb)
