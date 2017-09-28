---
published: true
title: 'AWS Deep Learning AMI with Keras'
date: '2017-09-28 08:50:53 +1000'
excerpt: A few tips for using the AWS Deep Learning AMI
draft: false
layout: post
---

I was previously using the AMI from the [fast.ai](https://github.com/fastai/courses/tree/master/setup) course for my deep learning needs,
but recently decided to try out the [AWS Deep Learning AMI](https://aws.amazon.com/marketplace/pp/B06VSPXKDX) and I wanted to document a few 
tips for a few issues I encountered when I first set it up.

### Keras

The AWS Deep Learning AMI does not come with the latest version of Keras, so you'll need to update the keras package using:

`sudo pip install keras --upgrade`

Or, if you're using Python 3, you can update it using `pip3` instead:

`sudo pip3 install keras --upgrade`

You'll also need to remove older Keras configurations (if any) using:

`rm ~/.keras/keras.json`

### CUDA

I ran into some issues with CUDA initially, which turned out to be a configuration problem. If you encounter 
the following error (or something similar):

```
ImportError: libcusolver.so.8.0: cannot open shared object file: No such file or directory
Failed to load the native TensorFlow runtime.
See https://www.tensorflow.org/install/install_sources#common_installation_problems
for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.
```

Try updating the `LD_LIBRARY_PATH` using the following:

`sudo ldconfig /usr/local/cuda/lib64`

### Bonus: Autostarting Jupyter Notebook

Since I usually turn off my instance when I'm not using it, I wanted to autostart Jupyter whenever the instance was re-started 
to avoid SSHing everytime to start it up manually. Luckily, there's a way to do that!

Create a new file called `jupyter_start.sh` in your home directory, and add the following:

```
export PATH="$PATH:/home/ubuntu/anaconda2/bin"
jupyter notebook --notebook-dir=/home/ubuntu/ --profile=nbserver > /tmp/ipynb.out 2>&1 &
```

Change `anaconda2` to `anaconda3` if you're using Python 3. You can also change `notebook-dir` to the location of your Jupyter notebooks. 

Next, edit the `/etc/rc.local` file and add the following line before `exit 0`:

```
su ubuntu -c 'bash /home/ubuntu/jupyter_start.sh'
```

Let me know if you encounter any issues, or have any useful tips for using the AWS Deep Learning AMI!
