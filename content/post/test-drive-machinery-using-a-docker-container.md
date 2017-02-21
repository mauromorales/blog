---
title: Test Drive Machinery Using a Docker Container
date: 2015-09-10
published: true
featured: 1
---

In my [last post][1] I shared how to install Machinery. The process assumes that
you are running openSUSE 13.2 but this of course might not be the case. You
might be running a different Linux distro or even another OS. That's were Docker
comes very handy. In this post I will show you how to easily test drive
Machinery on a container.

READMORE

Machinery will save all its data in a `.machinery` folder at your home
directory. In order to be valuable, this data needs to persist after you kill
your Docker containers.

```shell
mkdir -p ~/.machinery
```


Now we can pull the image from the Docker Hub

```shell
docker pull mauromorales/machinery
```

Once the image is in your system all we need to do is to start a container every
time we want to run machinery.

```shell
$ docker run -ti \
> -v ${HOME}/.machinery:/root/.machinery \
> -v ${HOME}.ssh:/root/.ssh \
> mauromorales/machinery  /bin/bash
```

Here we are telling docker to start a container in `-ti` interactive mode with
tty and to `-v` map a volume from the folder we just created to the
`/root/.machinery` this is root's home directory because root is running
Machinery inside our container. Additionally we will also map our `.ssh` folder
so machinery can use your ssh keys to access the remote servers (if you don't
want to share this folder with your container just map it to a different
directory and generate new keys) `mauromorales/machinery` specifies the image we
downloaded and finally we run bash.

Once your container has started you can start using Machinery :) happy hacking!

[1]:/blogs/installing-machinery/

