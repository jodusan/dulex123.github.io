---
layout: post
title: Docked deep learning
category: posts
summary: Avoid bloating your system with tons of dependencies.
---

*If you are tired of installing bloatware everytime you want to run a project that has dependencies, then Docker may be a solution for you!*

___

Docker allows you to isolate your new project environment from your host OS efficiently, something like virtualenv in python but more VM-like, but then again without the whole OS installation and massive resource consumption.

## 1. Theory

So let's start with easy docker concepts:

- **Container**  
  All action happens within the container. When you want to run a script you run it from the inside of a container, if you want to install software, you run it inside the container. If your friend wants to try your software without wasting 14hrs on framework stack setup, you send him your container.

  So the container is an environment in which you can do whatever you want with no consequences to your host OS. And if you screw something up within a container? You just delete it, create another one (or 6, 10, 200) just like it and try again.

  So container idea as a separate entity should be clear by now. But what is within a container? What software is preinstalled? Which OS?


These questions lead us to *the image*.

- **Image**
  Image is an immutable template that can be instantiated as a container. Something like this:

![Image container relationship](http://i.imgur.com/xZr1QfY.png)

  So once created, an image is immutable (changes during the lifetime of a container does not affect the image). It serves as a common ground for all containers it instantiates. Whatever you do is saved within a container, but container communicates with its image and all operations are going through the image. You need one image, and you can instantiate any number of containers.

  How this helps is evident from the following:

  *If you had a 2GB VM, you would need 2GB x how many VMs you want. And that's only disk space; there will be massive consumption of CPU, RAM and other resources you need. With Docker, much of that 2GB are shared between containers, and with 1000 containers you will get only a little over a 2GB space requirement. (If they are of the same image ofc) So for less isolation than an actual VM you are getting very lightweight containers.*

  Also, fully virtualized VM takes minutes to start where Docker container takes only seconds.

  An alternative explanation of image and container relationship from OOP perception:  
  - Image is equivalent to a class in OOP whereas  
  - A container is equivalent to an object

But what defines an image? How is it created? - It comes from a  txt file called:

- **Dockerfile**
  Textual file on what to setup within an image that can later be instantiated as a container and used. You build docker image from dockerfile.

  The beautiful thing is that probably someone somewhere before you needed the same or similar stack and there are docker files for you just to download them. Even better, instead of building an image yourself (which depending on a dockerfile can last for quite some time), you can find them online at dockerhub or similar websites, download it, instantiate a container and start experimenting with your code.


Although containers are isolated, you can share folders between host OS and container, open network ports, etc., just as in regular VM.

## 2. Practice
So let's use all this information to create a suitable environment for a deep learning project. My host OS is Ubuntu 16.04 so that's what I'll use.

- Step 0: Install docker and nvidia-docker
  - [Official docker installation][docker-official]
  - [Nvidia-docker][nvidia] package is used to leverage GPU for anything you do inside container, you just need to have nvidia drivers installed on your host OS. It is not necessary, but for our application is very desirable. If you are not using GPU you don't need this package. Everywhere you see nvidia-docker just type docker instead.

- Step 1: Getting suitable dockerfile
  - I've stumbled upon this great dockerfile repo [dl-docker]. It will install Ubuntu 14.04 with CUDA 8, cuDNNv5, Tensorflow, Caffe, Theano, Keras, Lasagne, Torch, iPython/Jupyter, Numpy-Scipy-Scikit learn-matplotlib on top of it.

    To get the dockerfile from github just type:

  ~~~ bash
  $ git clone https://github.com/saiprashanths/dl-docker.git
  $ cd dl-docker
  ~~~

- Step 2: Build or download image
  - Since many images are available for download, you can directly download your image from a place like [dockerhub]. We are going to build our own image because currently there is no GPU image for dockerfile on dockerhub. Check the bottom of this page if you run into errors:

  ~~~ bash
  # Notice the dot at the end
  $ docker build -t floydhub/dl-docker:gpu -f Dockerfile.gpu .
  ~~~

   *There is an image on dockerhub without GPU support and we could download it without building*

  ~~~ bash
  # We don't need this if we are using GPU
  $ docker pull floydhub/dl-docker:cpu
  ~~~

- Step 3: Instantiate a container
  - Now we will create our container with two network ports and one folder shared with the host OS.

  ~~~ bash
  nvidia-docker run -it -p 8888:8888 -p 6006:6006 -v /sharedfolder:/root/sharedfolder floydhub/dl-docker:gpu bash
  ~~~

  Parameter **-p port1:port2** maps port1 from host OS to port2 on container  
  This means that whatever is launched on port2 inside container will show up in your host OS browser if you go to http://localhost:port1.

  Parameter **-v folder1:folder2** maps absolute path folder1 on host OS to folder2 on container. If you change anything in folder1 it will reflect inside of a container on folder2.

Once you create a container, you will get a root access to it (no need for users since it is isolated right?). When you finish your work, you can type exit, and it will save the container state and stop the container. You will probably want to use that container again so here are some commands that can be helpful:

~~~ bash
# List all images
$ sudo docker images

# List currently running containers
$ sudo docker ps

# List all containers
$ sudo docker ps -a

# Start some container
$ sudo docker start <container_name>

# Connect to container (Attach to container terminal)
$ sudo docker attach <container_name>

# Connect second terminal to container (or more)
$ sudo docker exec -it <container_name> bash

# Remove a container
$ sudo docker rm <container_name>

# Remove an image
$ sudo docker rmi <image_name>

# You can use container_id instead of container_name

~~~

 **Note:** I've said that you can ship containers to a friend. Although this is possible much more sensible thing to do would be for him to download/build an image and pull your project from a git repo, within the instantiated container.


You may also find useful this [beautiful cheatsheet][cheatsheet] that has many
useful docker commands which can help your docker-fu.

## Errors while building the image:

- setup.py egg_info error can be solved with  

  ~~~ bash
  pip install setuptools==33.1.1
  ~~~
- If you get an error with libopenjpeg2, the package is renamed to libopenjpeg5 so you just need to:

  ~~~ bash
  sudo apt-get install libopenjpeg5
  ~~~



[docker-official]: https://docs.docker.com/engine/installation/linux/ubuntu/
[nvidia]: https://github.com/NVIDIA/nvidia-docker
[dl-docker]: https://github.com/floydhub/dl-docker
[cheatsheet]: https://github.com/wsargent/docker-cheat-sheet/blob/master/README.md
[dockerhub]: https://hub.docker.com/
