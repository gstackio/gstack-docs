---
layout: default
language: en
title: Your app in Gstack context
tagline: <em>“Do you want any ham with your mashed potatoe?”</em> <span class="nowrap">– B. Gandon</span>
description:
---

When you push a Docker container to Gstack, there is few mystery about how
Gstack runs it. It just brings the conntainer up and runs it.

For applications, it's different. There is obviously more processing involved
before they are packaged into a runnable container.


## The stack below your application

After you `gk push` an application to Gstack, three layers will be stacked to
bring it online, up and running.

1. A Warden container
2. A root filesystem (usually misnamed
   “[Stack](https://docs.cloudfoundry.org/concepts/stacks.html)”)
3. A buildpack, that provides compilation, packaging, and runtime

We detail these 3 layers in reverse order, starting with the “Buildpack” layer
that is the nearest from Gstack applications.


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

When using Gstack, you can see which root file systems are available with the
command `gk stacks` (indeed, root filesystems are misnamed “stacks” in Cloud
Foundry). New versions are released [here](https://github.com/cloudfoundry/stacks/releases)
from times to times. Only a Gstack admin can deploy a new version of a root
filesystem.


### A Warden Container

Why not just [Docker](https://www.docker.com/)?

> <a href="https://en.wikipedia.org/wiki/TL;DR">TL;DR</a>: Actually it
> makes little difference because Warden and Docker are very similar
> technologies. Created nearly at the same time, they evolved
> targetting different audiences. Even if Warden is provided for
> historical reasons, it still fits best the Gstack use cases.

Gstack is based on Cloud Foundry and historically, Cloud Foundry started
creating its own container technology, called
“[Warden](https://github.com/cloudfoundry/warden)”, nearly at the same time
Docker did. They just didn't met the same level of popularity.

Before Docker reached its outstanding success, Warden and Docker were both
obscure technologies, both following their own path as they were targeting
different audiences. To make long story short, Docker focused on human
end-users, whereas Warden was created to be controlled by machines, through an
external API.

For example, Docker said that containers would not control their network
settings because in their use cases, it was easier for end-users to have the
container engine impose consistent network settings to containers. Warden
didn't do that because it didn"t make sense in its context. In turns, Warden
had to be managed by some external API, so that many Warden backends can be
managed by a central “brain” called “Cloud Controller”. An external API was
for sure no priority to Docker at the time, as a consequence of the intended
focus on real-user interactions.

This explains why applications that are pushed on Gstack (using a buildpack),
are deployed in Warden containeres. And now developers can also push full-
blown Docker containers in Gstack.


## Further readings

Now that you know how applications are deployed in Gstack, you might be
interested in [writing your own](../create-application).
