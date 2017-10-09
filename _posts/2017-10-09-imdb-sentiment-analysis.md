---
published: false
title: 'IMDb Sentiment Analysis with Keras'
date: '2017-09-28'
excerpt: A few tips for using the AWS Deep Learning AMI
layout: post
---

I was previously using the AMI from the [fast.ai](https://github.com/fastai/courses/tree/master/setup) course for my deep learning needs,
but recently decided to try out the [AWS Deep Learning AMI](https://aws.amazon.com/marketplace/pp/B06VSPXKDX) and I wanted to document a few 
tips for a few issues I encountered when I first set it up.

### Models 

The AWS Deep Learning AMI does not come with the latest version of Keras, so you'll need to update the keras package using:

`sudo pip install keras --upgrade`

Or, if you're using Python 3, you can update it using `pip3` instead:

`sudo pip3 install keras --upgrade`

You'll also need to remove older Keras configurations (if any) using:

`rm ~/.keras/keras.json`

### Deploying the Keras Model


