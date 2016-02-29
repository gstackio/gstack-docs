---
layout: default
language: en
title: Getting started with Gstack
tagline: <em>“Every project should onboard newcomers in a snap”</em> – B.&nbsp;Gandon
description:
---
## Get the Gstack command-line tool

To install the `gk` command, just run:

    curl -fsSL http://get.gstack.io/please | sh

or:

    wget -qO- http://get.gstack.io/please | sh

Supported platforms are: Linux (64 or 32 bits) and OS X (64 bits only).
Windows users need to download [here](https://github.com/cloudfoundry/cli#downloads)
the original `cf` tool (called “[CLI](https://en.wikipedia.org/wiki/Command-line_interface)”).

Gstack *is* a Cloud Foundry deployment—no secret about that. So for now, using
the standard `cf` utility is exactly the same, because the `gk` command is just
a swaggish rename of it.


## Select the Gstack API endpoint and login

First select the API endpoint for Gstack:

    gk api --skip-ssl-validation https://api.gstack.me

Then just login with `gk login`, and the tool will ask you your credential
like this:

    $ gk login
    API endpoint: https://api.gstack.me

    Email> benjamin

    Password> ********
    Authenticating...
    OK

Easy as that.


## Get help about the `gk` command

For inline help, just type `gk help`.

As Gstack is based on Cloud Foundry, and the `gk` command just a mere rename
of the standatd `cf` utility, you'll find any help you need in the standard
[Cloud Foundry docs](https://docs.cloudfoundry.org/cf-cli/).


## Push your first app

Before you can `gk push` your first app, you first need to:

1. Write an application.
2. Write a Cloud Foundry `manifest.yml` file, containing just a few YAML
   properties.
3. Comply to some Cloud Foundry conventions, depending on you app technology
   (Ruby, Java, etc).

Ready? Let's go!


### Write an application

This subject is beyond the scope if this *Getting Started* guide. Depending on
which language you are familiar with, we recommend you follow one of our
detailed step-by-step tutorials. Those will guide you in writing web applications
that follow state-of-the-arts best practice.

 - [Java](../create-application/java)
 - [Ruby](../create-application/ruby)
 - [Node](../create-application/node)
 - [PHP](../create-application/php)
 - [Go](../create-application/golang)
 - [Python](../create-application/python)


### Write your `manifest.yml`

More to come in this section. PRs are welcome!

In the meanwhile, go read the
“[Deploying with Application Manifests](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)”
section of the stock Cloud Foundry docs.


### Comply with Cloud Foundry conventions

More to come in this section. PRs are welcome!

Here are a few of those conventions:

 - Ruby apps need a `config.ru` file, and recommend you also provide a
   `Procfile`.

 - For correct database migrations in Ruby apps, you'll need a
   `cf:on_first_instance` trivial Rake task.

 - The PostgreSQL-BDR service provided by Gstack is based on BDR, which
   unfortunately has tough limitations. Expecially, database migrations with
   combo `ALTER TABLE … ADD COLUMN … DEFAULT` statements will be rejected.
   You'll have to spli those into separate `ALTER TABLE … ADD COLUMN` and
   `ALTER TABLE … SET DEFAULT` statemente. We currently have no easy solution
   to that but if you find one, we are please to hear!


### Ready to `gk push`?

That's it, you're ready.

If you wonder how to use `gk push`, just type `gk push --help`. You'll find
more information in the “[Develop and Manage Applications](https://docs.cloudfoundry.org/devguide/)”
section of Cloud Foundry docs.


## Further readings

After you `gk push` your app, Gstack will create an “HTTP route”, so that you
can access it with your web browser. Read our [HTTPS Guide](../https-routes) to
implement an encrypted “HTTPS route”.

To plug a database into your app, see our [Services Guide](../plugging-services).
You'll learn how those services shall find their way into your
`manifest.yml` deployment descriptor.
