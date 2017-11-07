---
published: true
title: 'AWS Deep Learning AMI with Keras'
date: '2017-09-28'
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

### Jupyter Notebook Configuration
Generating the default configuration for Jupyter notebook can be done using: `jupyter notebook --generate-config` and to avoid using the token, you may be interested in setting up a password instead using: `jupyter notebook password`

There were other issues I encountered with running this on EC2 which was actually getting the notebook up and running in the browser. If you had issues connecting to the notebook via the browser, you may find the following useful:

If you've generated a password, there should be a `jupyter_notebook_config.json` where you can set user-specific settings. Alternatively, you can also try modifying `jupyter_notebook_config.py`. Here's an example of my JSON configuration:
```
{
  "NotebookApp": {
    "ip": "*",
    "open_browser": false,
    "password": "PASSWORD_HASH"
  }
}
```

In order to get the notebook up and running in the browser, you will either need to adjust the security group associated with your EC2 instance or utilise SSH tunneling.

**Security Group**

Edit the security group for your EC2 instance and add a new rule to allow the following ports: `8888-8898` with TCP as the protocol and `0.0.0.0/0` as the source for the rule. You should now be able to access your notebooks via http://EC2_INSTANCE_IP:8888/

**SSH Tunnelling**

Alternatively, you can use SSH tunnelling which will let you open the notebook via localhost by binding the port locally. This can be achieved using `ssh -i ~/.ssh/<KEY_NAME.pem> ubuntu@<EC2_INSTANCE_IP> -L 8888:127.0.0.1:8888` where `KEY_NAME` is the name of your keypair that you downloaded from Amazon, and `EC2_INSTANCE_IP` is the IP address of your instance.

You should now be able to access the notebook using http://127.0.0.1:8888/. Please note that you'll need to maintain the SSH connection for this to work properly.

#### Bonus: Autostarting Jupyter Notebook

Since I usually turn off my instance when I'm not using it, I wanted to autostart Jupyter whenever the instance was re-started 
to avoid SSHing everytime to start it up manually. Luckily, there's a way to do that!

Create a new file called `jupyter_start.sh` in your home directory and make sure it can be executed, and add the following:

```
export PATH="$PATH:PATH_TO_ANACONDA"
jupyter notebook --notebook-dir=/home/ubuntu/ --profile=nbserver > /tmp/ipynb.out 2>&1 &
```

- For AWS Deep Learning AMI, change `PATH_TO_ANACONDA` to `/home/ubuntu/anaconda3/bin`
- For fast.ai AMI, change `PATH_TO_ANACONDA` to `/home/ubuntu/src/anaconda3/bin`.

You can also change `notebook-dir` to the location of your Jupyter notebooks. 

Next, edit the `/etc/rc.local` file and add the following line before `exit 0`:

```
su ubuntu -c 'bash /home/ubuntu/jupyter_start.sh'
```

Let me know if you encounter any issues, or have any additional useful tips!
