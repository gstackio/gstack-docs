---
layout: default
language: en
title: "Gstack Documentation"
tagline: "Because understanding is critical for a developer"
description: ""
---
### Download the command-line tool

To install the `gk` command, just run:

    curl -fsSL http://get.gstack.io/please | sh

or:

    wget -qO- http://get.gstack.io/please | sh

Supported platforms are: Linux (64 or 32 bits) and OS X (64 bits).


### Select the Gstack API endpoint and login

First select the API endpoint for Gstack:

    gk api --skip-ssl-validation https://api.gstack.me

Then just login, answering the questions asked by the tool:

    gk login


### Get help about the `gk` command

For inline help, just type `gk help`.

Beyond that, Gstack is based on Cloud Foundry, there is no secret about that.
So for now, the `gk` command is just a mere rename of the standatd `cf`
utility. If you need help, you'll find all sorts of details in the standard
[Cloud Foundry docs](https://docs.cloudfoundry.org/cf-cli/).


### Push your first app

Before you push your first app, you need to:

1. Write an app
2. Write a Cloud Foundry manifest file, containing just a few YAML properties
3. Comply to some Cloud Foundry conventions, depending on you app technology
   (Ruby, Java, etc)

Once you are there, you can push you app with `gk push`.

This needs services (dasabase especially) and an HTTP/HTTPS route. You'll have
to setup these and specify their name in your `manifest.yml`.

If you wonder how to use `gk push`, just type `gk push --help`. You'll find
more information in the “[Develop and Manage Applications](https://docs.cloudfoundry.org/devguide/)”
section of Cloud Foundry docs.


### Happy Gstack!

You're good to go. Never forget that Google is your friend. ;-)

