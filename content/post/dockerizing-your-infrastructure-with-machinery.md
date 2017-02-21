---
title: Dockerizing Your Infrastructure with Machinery
date: 2015-10-15
tags: machinery, docker, introspection, vm, container
published: true
---

Docker offers to make it easy to deploy applications no matter what you
infrastructure is. The problem is that you cannot just switch from one
technology to another overnight just because it will help you solve certain
issues. Moving to the new technology is an issue on its own, plus there is no
technology that doesn't come with its own issues. With Machinery we want to help
you make the transition from VMs to containers as smooth as possible.

READMORE

_This post was originally delivered as a presentation in LinuxCon Dublin 2015._

Let's assume you have a server running a web application that uses Rails, Apache
and MariaDB. We will use Machinery to create a System Description from this
server which we will manually review and then allow machinery to automatically
detect the respective workloads. Finally we will use machinery to verify that
the see the differences between the original system and the resulting
containers.

On my case I will use [Portus](http://suse.github.io/Portus/) as my Rails
application.

Our work flow will look more or less like this:

![Work flow](dockerizing-your-infrastructure-with-machinery/workflow.png)

## Creating a system description

First of all you will need to make sure you can access the server without
requiring a password by copying your ssh keys into the server.

```
ssh-copy-id root@192.168.1.100
```

This is required because machinery will open an ssh connection and run a set of
commands in order to evaluate the information inside the server. Then go ahead
and inspect the server in order to create a system description.

```shell
machinery inspect --name portus --extract-files 192.168.1.100
```

The option `--name` is the name we want to give to our system description. The
option `--extract-files` will make a copy of every file except those that are
the default from an RPM package.

Once machinery has finished a folder named `wordpress` will be created inside
`/home/USERNAME/.machinery`. If you have a pick into this folder you will find
a file called `manifest.json` which is the physical representation of the system
description and a `files` directory containing the extracted files.

You can easily open the system description with your favorite text editor but
machinery also provides a `show` command for easy access.

```shell
machinery show portus --html
```

The additional `--html` option will tell machinery to start a web server on your
local machine which will help us have a more interactive look about how this
server has been set up. By omitting this option you will get access to the same
information as text through the CLI.

Your default browser will open up with all the information about the wordpress
system description. From here you can manually see in detail every configuration
file, package and service that was running in order to write your Dockerfile.


![HTML View](dockerizing-your-infrastructure-with-machinery/html-view.png)

This feature on its own will help you out a lot because you can translate all
this information not only into a Dockerfile but also into a configuration
management system.

## Containerizing

For the particular case of Rails (using MariaDB and Apache2) we've implemented
a really nice feature into machinery that will allow you to only run a command
and get the Dockerfiles created for you and also the docker-compose files so you
can orchestrate your application once it has been containerized.

In order to do this you need to tell machinery to containerize a system
description whose files have been extracted (in our case the wordpress system
description)

```shell
machinery containerize --output-dir /tmp/containers portus
```

The required `--output-dir` tells machinery where you want your containerized
version of the server to be saved.

Now we can go to `/tmp/containers/portus` and have a look. Machinery has
created a set of files for us to run a containerized version of our application.
The details on how to set it up and get it running will be inside the
`README.md` file.

The first thing we need to do is to run the setup script.

```shell
./setup.rb
```

This script is unique depending on the application that was containerized. In
the case of Portus it sets up a database container so that our web container can
connect to it.

```shell
docker-compose up
```

Docker compose will start an instance of all the required containers. For Portus
this means a web, a db and a registry container and allow you to access them
from your local machinery through port 3000.

Voila! You just got yourself a containerized version of your application. How
cool is that?

## Introspection

Now this sound very nice but once you've done all this (specially if you are
writing your own Dockerfiles or CMS recipes, modules, etc) you might want to
check what is inside your resulting containers.

```shell
machinery inspect-container portus_db
```

At this point I could just `show` the `portus_db` system description and have
a look if everything is the way I expect it to be but we can go one step further
and compare it to the `portus` system description. This way I can make sure
what the differences are between the container and the VM.

```shell
machinery compare portus portus_db
```

I could have added the `--html` option but I won't so you can see the cli
view of machinery. It was designed to play well with your other \*nix commands
so you could easily do something like this, to determine if there is
a difference in the MariaDB package.

_If there were no differences machinery would ommit this information. For this
reason I've created a contianer with a different MariaDB version_

```shell
machinery compare portus portus_db --no-pager | grep mariadb
* mariadb (version: 10.0.21 <> 10.0.20)
* mariadb-client (version: 10.0.21 <> 10.0.20)
* mariadb-errormessages (version: 10.0.21 <> 10.0.20)
```

You can easily spot the differences here. Now it's up to you to change your
Dockerfile or CMS files in order to add the right version of the package you'd
like to install.

## Summary

As you can see Machinery can be very useful to start deploying your applications
as docker containers. Inspect an existing system and understand how it was
configured using the `inspect` and `show` commands respectively. Write your
Dockerfiles and build your docker images which you can then also inspect and
compare to the original system using the `inspect-container` and `compare`
commands.

While doing this remember to have a lot of fun and share back how you used
machinery in your own project.
