---
title: Installing Machinery
date: 2015-08-11
tags: machinery, opensuse
published: true
featured: 4
---

For the past three months I've been working on a really cool tool called
[Machinery][1] and I'd like to share some of the neat things you can do with it.

READMORE

In this first post I'll show you how to install Machinery on [openSUSE
13.2](/blog/run-opensuse-13-2-in-linode)

First you need to add the `systemsmanagement` repo to your system. You can
easily do it with zypper running the following command as root:

```shell
zypper ar -f http://r.opensu.se/systemsmanagement:machinery/13.2 \
systemsmanagement
zypper refresh
```

_You will get asked about a key for the repo. At this point you need to trust
temporarily or trust always._

Immediatelly afterwards we should be able to install Machinery by running:

```shell
zypper in machinery
```

_When asked if you want to install the respective packages you'll need to answer
with a "y" (yes)_

Now let's double check that machinery was propperly installed by running the
version option.

```shell
machinery --version
```

At the moment of writing the version is:

```shell
machinery version 1.11.2 (system description format version 4)
```

Learn more about how to use Machinery on the [wiki][2] or by reading the man
page.

```shell
man machinery
```

Have fun! And if you spot any issues please let us know by opening an [issue][3]
and if you have any questions write us to our [mailing
list](mailto:machinery@lists.suse.com)

[1]:http://machinery-project.org
[2]: https://github.com/SUSE/machinery/wiki
[3]: https://github.com/SUSE/machinery/issues

