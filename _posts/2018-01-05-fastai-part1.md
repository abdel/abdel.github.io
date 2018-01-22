---
published: true
title: 'Key Takeaways from fast.ai Part 1'
date: '2018-01-05'
excerpt: Insights & best practices from the course
layout: post
---
I recently completed the new edition of the [fast.ai](http://course.fast.ai/) Part 1 course which was available for individuals who were accepted into the International Fellowship program. Throughout the course, we got to not only build state-of-the-art for models for image classification, language, sentiment analysis, structured data and recommender systems, but also understand what’s going on behind the scenes. I thought it would be useful share some useful insights and best practices that I was not really aware of prior to the start of the course.

## Cyclical Learning Rates
Smith (2015) described a new method in [1], cyclical learning rates, for setting the learning rate without the need to experimentally find the best values for the global learning rates. Alternatively to a monotonically reducing the learning rate, this new method varies learning rate between reasonable boundary values. The paper also introduced a way to estimate these reasonable boundaries.

The fast.ai library takes advantage of this technique and allows you to easily search for an optimal learning rate, see the folllowing code snippe or this [introductory notebook](https://github.com/fastai/fastai/blob/master/courses/dl1/lesson1.ipynb) from course.

	# Setup model
	learn = ...
	
	# Find optimal learning rate
	lr_finder = learn.lr_find()
	
	# Plot learning rates against iterations
	learn.sched.plot_lr()
	
	# Plot loss against learning rates
	learn.sched.plot()

## Stochastic Gradient Descent with Restarts
Building on work from Smith and others, Loshchilov & Hutter (2016) introduced "Stochastic Gradient Descent with Warm Restarts" in [2]. The method introduced in the paper, cosine annealling, where the learning rate is decreased along the cosine function. This repeats in cycles, where at the start of each cycle it starts with the maximum value. A cycle can be made longer than the previous cycles by using a constant factor `T_mul`.

This is then taken further by Huang, Li and Pleiss (2017) in [3] to get even better results with an ensemble of neural networks at the cost of one. This is achieved by saving the weights after each cycle in SGDR, and add N networks to the ensemble based on the last N cycles. Essentially the model “undergoes several learning rate annealing cycles, converging to and escaping from multiple local minima".

This also already implemented in the fast.ai library, and can be easily done during training:

	learn.fit(learning_rate, epochs, cycle_len=1, cycle_mult=3)`

## Fine-tuning for Text Classification
A common technique used in creating text classification models is using standard pre-trained word embeddings, such as GloVe and word2vec. An alternative approach introduced in the [fourth lesson](https://github.com/fastai/fastai/blob/master/courses/dl1/lesson4-imdb.ipynb) is to create a language model based on the training dataset you intend to use, so the model can first understand the structure of the dataset before using it for classification. The vocabulary and encoder portion of the language model can then be fine-tuned for the classification task, in this case, analysing sentiment analysis on IMDb movie reviews. I am planning a follow up to my [previous article](https://abdel.me/2017/11/06/imdb-sentiment-analysis/) on sentiment analysis to explore this further and improve my existing model.

**Update (18/01/2018):** This technique was explored further in a recent paper "Fine-tuned Language Models for Text Classification" by Howard and Ruder [4].

## Common Techniques
As mentioned previously, the focus of the article was on key topics that are relatively new. However, it’s still worth highlighting the following topics as they’re important and have become standard practices available in most of the available frameworks.

### Transfer Learning
Transfer Learning transformed the field of computer vision, which I have [previously](https://i.abdel.me/qtM8V2s4Zp/Continuous_Learning.pdf) mentioned. This approach works by utilising an existing neural network architecture, such as ResNet, that was trained on a different problem, in this case ImageNet. The pre-trained model can then be ‘fine-tuned’ on a different problem, by ‘freezing’ the earlier layers and replacing the last layer to fit your problem. For example, taking an ImageNet model that classifies 1000 categories, and changing the last layer to classify 2 categories. There is a trade-off with unfreezing more layers,  depending the architecture and problem, as the more you unfreeze, the more learned features you are losing from the pre-trained model.

### Data Augmentation
Data augmentation is a commonly used method in computer vision for increasing the number of images in the training and test sets. The kind of transformations that should be performed on the image depends on the type of problem. Common transformation techniques include rotation and zooming. Data augmentation can also be performed at test-time.

## Conclusion
I would definitely recommend checking out [fast.ai](https://course.fast.ai/) if you’re interested in a more practical approach to deep learning. I would also recommend additional resources alongside it for a better theoretical foundation, such as [CS231n](http://cs231n.stanford.edu) and [Deep Learning Book](http://www.deeplearningbook.org). It’s also worth mentioning that [applications are open](http://www.fast.ai/2018/01/17/international-spring-2018/) for the new fast.ai Part 2 International Fellowship until the 28th of February 2018.

## References
[1] Smith, L. N. 2015, Cyclical Learning Rates for Training Neural Networks, *arXiv preprint [arXiv:1506.01186](https://arxiv.org/abs/1506.01186)*.

[2] Loshchilov, I. & Hutter, F. 2016, Stochastic Gradient Descent with Warm Restarts, *arXiv preprint [arXiv:1608.03983](https://arxiv.org/abs/1608.03983)*.

[3] Huang, G., Li, Y. & Pleiss, G. 2017, Snapshots Ensemble: Train 1, get M for free, *arXiv preprint [arXiv:1704.00109](https://arxiv.org/abs/1704.00109)*.

[4] Howard, J. & Ruder, S. 2017, Fine-tuned Language Models for Text Classification, *arXiv preprint [arXiv:1801.06146](https://arxiv.org/abs/1801.06146)*.
