---
published: true
title: 'IMDb Sentiment Analysis with Keras'
date: '2017-11-06'
excerpt: Taking a small project from exploration to production
layout: post
---

I was introduced to Keras through the [fast.ai](http://course.fast.ai/) Part 1 course, and I really enjoyed using it. I was interested in exploring it further by utilising it in a personal project. I had an opportunity to do this through a university project where we are able to research a machine learning topic of our choice. I decided leverage what I learned from the fast.ai course, and explore and build a model for sentiment analyis on movie reviews using the [Large Movie Dataset](http://ai.stanford.edu/~amaas/data/sentiment/) by Maas et al. I also wanted to take it a bit further, and worked on deploying the Keras model alongside a web application.

### Data Exploration & Preparation
The dataset contains 50,000 movie reviews in total with 25,000 allocated for training and another 25,000 for testing. Some basic data exploration was performed to examine the frequency of words, and the most frequent unigrams, bigrams and trigrams. This was useful to kind of get a sense of what really makes a movie review positive or negative.

![Word cloud showcasing word frequencies](/assets/imdb_sentiment_wordcloud.png)

The dataset was converted to lowercase for consistency and to reduce the number of features. The movie reviews were also converted to tokenized sequences where each review is converted into words (features). The word frequency was identified, and common stopwords such as 'the' were removed. A dictionary was then created where each word is mapped to a unique number, and the vocabulary was also limited to reduce the number of parameters. Additional sequence processing techniques were used with Keras such as sequence padding.

### Modelling
I experimented with different model architectures: Recurrent neural network (RNN), Convolutional neural network (CNN) and Recurrent convolutional neural network (RCNN). The RCNN architecture was based on the [paper](http://www.nlpr.ia.ac.cn/cip/~liukang/liukangPageFile/Recurrent%20Convolutional%20Neural%20Networks%20for%20Text%20Classification.pdf) by Lai et al. in which they aim to combine the benefits of both architectures, where the CNN can capture the semantics of the text, and the RNN can handle contextual information.

The models were trained on an Amazon P2 instance which I originally setup for the fast.ai course. I experimented with a number of different hyperparameters until a decent result was achieved which surpassed the model by Maas et al. Using my configurations, the CNN model clearly outperformed the other models.

![Model results](/assets/imdb_sentiment_results.png)

The model architectures and parameters can be found in the Jupyter notebooks on the [GitHub repository](https://github.com/abdel/imdb-sentiment-analysis/tree/master/notebooks).

### Deploying the Keras Model
I was interested in exploring how models would function in a production environment, and decided it was a good opportunity to do this in the project (and potentially get some extra credit!). I stumbled upon a great [tutorial](https://medium.com/@burgalon/deploying-your-keras-model-35648f9dc5fb) on deploying your Keras models by Alon Burg, where they deployed a model for background removal. The CNN model configuration and weights using Keras, so they can be loaded later in the application.

Loading the model was is quite straight forward, you can simply do:

```
import tensorflow as tf
from keras.models import load_model

model = load_model('./model/cnn_model.h5', compile=False)
graph = tf.get_default_graph()
```

It was also necessary to preprocess the input text from the user before passing it to the model. Similar preprocessing technique were performed such as lowercasing, removing stopwords and tokenizing the text data. The predictions can then be performed using the following:

```
text = preprocess(text)

with graph.as_default():
    pred_prob = model.predict(text)[0][0]
    pred_class = model.predict_classes(text)[0][0]
````

The web application was created using Flask and deployed to Heroku. The application accepts any text input from the user, which is then preprocessed and passed to the model. The model can then predict the class, and return the predicted class and probability back to the application. The predicted sentiment is then immediately shown to the user on screen.

![Preview of web application](/assets/imdb_sentiment_webapp.png)

The source code for the web application can also be found in the [GitHub repository](https://github.com/abdel/imdb-sentiment-analysis/tree/master/webapp). A demo of the web application is available on [Heroku](https://imdb-sentiment-analysis.herokuapp.com). Feel free to let me know if there are any improvements that can be made!

### Resources
- [Source Code](https://github.com/abdel/imdb-sentiment-analysis)
- [Project Document](https://i.abdel.me/3oDb3JTBcC/Sentiment-Analysis-on-Movie-Reviews.pdf)

### Credits

- [Deploying your Keras model](https://medium.com/@burgalon/deploying-your-keras-model-35648f9dc5fb)
