---
title: Run openSUSE 13.2 in Linode
date: 2015-06-26
tags: linode, opensuse, vps
published: true
featured: 5
---

Linode is one of my favorite VPS providers out there. One of the reasons why
I like them is because they make it extremely easy to run openSUSE.  This post
is a quick tutorial on how to get you started.

READMORE

The first time you log in you will be presented with the different Linode plans.
And the Location where your server will reside.

![1](run-opensuse-13-2-in-linode/linode-plans.png)

I'll choose the smallest plan

![2](run-opensuse-13-2-in-linode/linode-1024.png)

Once you see your Linode listed, click on it's name

![3](run-opensuse-13-2-in-linode/list-of-linodes.png)

Now click on "Deploy an image"

![4](run-opensuse-13-2-in-linode/deploy-an-image.png)

In there we will select openSUSE 13.2, the amount of space in disk. You can
leave the defaults which will choose for the full disk size with a 256MB swap
partition. Choose your password and click Deploy.

![5](run-opensuse-13-2-in-linode/select-opensuse.png)

This will take a bit but as soon as it's done you will be able to Boot your
machine.

![6](run-opensuse-13-2-in-linode/boot-your-linode.png)

Finally click on the "Remote Access" tab so you can see different options to log
into your machine.

![7](run-opensuse-13-2-in-linode/remote-access.png)

I personally like to ssh in from my favorite terminal app

```shell
ssh root@10.0.0.10
```

You will be welcomed by openSUSE with the following message:

```shell
Have a lot of fun...
linux:~ #
```

Now you can play with your new openSUSE 13.2 box. Enjoy!

BTW if you have never considered this Linux distro, here are [4 reasons
why](https://sysrich.co.uk/why-opensuse/) you will like it.
