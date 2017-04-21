---
layout: default
language: en
title: Your app in Gstack context
tagline: <em>“Do you want any ham with your mashed potatoe?”</em> <span class="nowrap">– B. Gandon</span>
description:
---
<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>

When you [push a Docker container](./getting-started#push-your-first-docker-container)
to Gstack, there is few mystery about how Gstack runs it. It just brings the
conntainer up and runs it.

When you push an applications to Gstack, it will also end up as a runnable
Docker container. But this one will be generated on the fly. We detail here
how this is done.


## The stack below your application

After you `gk push` an application to Gstack, three layers will be stacked to
bring it online, up and running.

1. A Docker **container**, that provides isolation to the runtime binaries
   that power your application.
2. A **root filesystem**, that provides basics for your application runtime to
   be compiled and run.
3. A **buildpack**, that provides compilation, packaging, and runtime.

We detail these 3 layers in reverse order, starting with the “Buildpack” layer
that is the nearest from your applications.


### A Buildpack

A “buildpack” is just a bunch of tools that are able to package your
application in a standard way, so that it can then be run. Gstack provides
some of the most popular buildpacks: [Go](../create-application/golang),
[Java](../create-application/java), [Node.js](../create-application/node),
[PHP](../create-application/php), [Python](../create-application/python),
and [Ruby](../create-application/ruby). Each one of these supports all the
major frameworks and dialects of the central technology it addresses.

The design of buildpacks is very simple, which participated to their
outstanding success.

 - A buildpack is either packaged as a __git repository__, or a __zip archive__.
 - It must provide these 3 executable scripts:
    1. `bin/detect` that tells whether the buildpack can build and run a given
       application.
    2. `bin/compile` that builds the application, after this has run, your
       application is completely ready to be started.

    3. `bin/release` that details how to run the application: which command
       line to execute, and which [environement variables](https://en.wikipedia.org/wiki/Environment_variable)
       to provide.
 - The 3 scripts above will be called in sequence.
    - As input, they take specific arguments.
    - As output, they return specific exit codes, print logs, or dump some
      YAML-formatted properties.

Buildpack make Gstack incredibly polyglot, because dozen of them have been
written so far. Even the most funkyest web technologies have a dedicated
buildpack! Besides the standard Gstack buildpacks, you can choose to use your
own very easily when you `gk push` you applications. You just need to specify
a mere Github URL on the command line or in the `manifest.yml`. Gstack will
then proceed downloading and using it for your app.

Buildpacks were first designed by Heroku. Considering their success, they were
later adopted by others and they are now available in Gstack. For more
details, go watch the [Heroku Buildpacks](http://talks.codegram.com/heroku-buildpacks)
presentation, it's brilliant.


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
You can see which root filesystems are available in Gstack with `gk stacks`.


### A Docker container

Your application as built, augmented and packaged by the buildpack will run
inside a Docker container.


## Further readings

Now that you know how applications are deployed in Gstack, you might be
interested in [writing your own](../create-application).

<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>
