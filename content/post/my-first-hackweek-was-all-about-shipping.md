---
title: My First Hackweek Was All about Shippping
date: 2015-12-19
tags: suse, yes_ship_it
published: true
---

Last week I had the chance to participate in my first [Hackweek][1]. I never had
such an experience in any other company I've ever worked for and between my
colleagues' reports about previous experiences and my own expectations I was
very excited to see what was all the fuzz about.

READMORE

These type of events are not unique to SUSE, as a matter of fact Twitter and
a bunch of other companies were also having their Hackweeks at the same time and
I'm glad this is the case because after having the chance to participate in one
I realize it's a great way to promote creativity.

> A hackweek is basically a week were you get to work on anything you want to
work on. You are not expected to deliver anything but instead encouraged to
experiment and explore with anything you think is worth spending time on.

In order to make the most out of Hackweek I decided to join a project and not
start one of my own so I could do some pairing. This kind of interactions always
make it a lot of fun for me plus I get to learn a ton.  That's how I joined
Cornelius Schumacher to work on [Yes Ship It!][2] This is a project he had
already started on his own so we were not doing everything from scratch.

> The approach of `yes_ship_it` is different from the typical release script. It
doesn't define a series of steps which are executed to make a release. It
defines a sequence of assertions about the release, which then are checked and
enforced.

The first thing we decided to do together was a Rails App which allows you to
track successful software releases. Since it was going to be 100% related to
`Yes Ship It!` we decided to call it [Yes It Shipped!][3]. Let me show you how
trivial it is to add it to a project like the [formstack-api gem][4].

  1. Install the `yes_ship_it` gem

    ```
    $ gem install yes_ship_it
    ```

  2. Add a `yes_ship_it.conf` file

    ```
    $ yes_ship_it init
    ```

  3. Release!

    ```
    $ yes_ship_it
    ```

By default `yes_ship_it` will check if:

  * you are in the right release branch (by default master) and the code was
     pushed.
  * the working directory is not missing to commit anything.
  * the version was update
  * the changelog was updated
  * a tag was added and published
  * a new version of the gem was built and published

The aim is to make it as generic as possible so you can adapt it to __any__ project you have. For starters you can remmove any check in the process and soon enough you will be able to add checks of your own.

What I like the most about it is that I can run `yes_ship_it` at any time.
I don't need to remember or make sure what was the last step I did because
that's exactly what it will do for me.

What do you think? Leave your comments below and remember to **release early
and release often**!

[1]: https://hackweek.suse.com/
[2]: https://github.com/cornelius/yes_ship_it
[3]: https://yes-it-shipped.herokuapp.com/
[4]: https://rubygems.org/gems/formstack-api
