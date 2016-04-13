---
layout: default
language: en
title: Your app in Gstack context
tagline: <em>“Do you want any ham with your mashed potatoe?”</em> <span class="nowrap">– B. Gandon</span>
description:
---

When you [push a Docker container](./getting-started#push-your-first-docker-container)
to Gstack, there is few mystery about how Gstack runs it. It just brings the
conntainer up and runs it.

When you push an applications to Gstack, it will also end up as a runnable
Docker container. But this container will be generated on the fly. We detail
here how this is done.


## The stack below your application

After you `gk push` an application to Gstack, three layers will be stacked to
bring it online, up and running.

1. A Docker **container**, that provides isolation to run the binaries that
   power your application.
2. A **root filesystem**, that provides basics for your application to compile
   and run.
3. A **buildpack**, that provides compilation, packaging, and runtime.

We detail these 3 layers in reverse order, starting with the “Buildpack” layer
that is the nearest from your applications.


### A Buildpack

A “buildpack” is just a package that packages your application in a standard
way, so that it can then be run.

Buildpack make Gstack incredibly polyglot, because dozen of them have been
written so far. Even the most funkyest web technologies have a dedicated
buildpack!

Gstack provides a bunch of the most popular buildpacks. But you can choose to
use your own very easily. Indeed, when you `gk push` you applications, you can
specify a custom buildpack, usually with a mere Github URL on the command line
or in the `manifest.yml`. Gstack will then proceed downloading and using it
for your app.

The design of buildpacks is very simple, which for sure participated to its
outstanding success.

 - A buildpack is either packaged as a __git repository__, or a __zip archive__.
 - It must provide these 3 executable scripts:
    1. `bin/detect` that tells whether the buildpack can build and run a given
       application.
    2. `bin/compile` that builds the application, after this has run, your
       application is completely ready to be started.
    3. `bin/release` that explains the system how to run the application, and
       especially the exact command line to run, and any required
       [environement variables](https://en.wikipedia.org/wiki/Environment_variable)
       to provide.
 - The 3 scripts above will be called in sequence.
    - As input, they take specific environment variables.
    - As output, they return specific exit codes, print plain text logs, or
      dump some YAML-formatted properties.

Buildpacks were first designed by Heroku back in… well… a long time ago.
Considering their success, they were later adopted by Cloud Foundry. For more
details, go watch the [Heroku Buildpacks](http://talks.codegram.com/heroku-buildpacks)
presentation. It's definitely brilliant.


### A Root Filesystem

The root filesystem is a `.tgz` archive containing a basic collection of
files. There are the base system utilities and configs that the buildpack and
the application will have access to.

For example, it provides basic C and C++ compilers, and script engines like
`bash`, `perl`, `python`, `ruby`, `sh`. This is meant to be enough for you to
download and build any missing technology, whether mainstream like
[Java](http://www.oracle.com/technetwork/java/javase/downloads/) or
[Go](https://golang.org/), or quite funky like [Pike](https://github.com/pikelang/Pike)
or [Rust](https://github.com/rust-lang/rust).

The standard root filesystem in Gstack is called `cflinuxfs2`. It is based on
a stable Ubuntu Linux distribution and contains something like 21,000+ files.

When using Gstack, you can see which root filesystems are available with the
command `gk stacks`.


### A Docker container

Your application as built, augmented and packaged by the buildpack will run
inside a Docker container.


## Further readings

Now that you know how applications are deployed in Gstack, you might be
interested in [writing your own](../create-application).
