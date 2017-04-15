---
layout: default
language: en
title: Getting started with Gstack
tagline: <em>“Every project should onboard newcomers in a snap”</em> <span class="nowrap">– B. Gandon</span>
description:
---
## Get the Gstack tooling

All you need to interact with Gstack is the `gk` command-line tool.

To install `gk`, just run:

    curl -fsSL http://get.gstack.io/please | sh

or:

    wget -qO- http://get.gstack.io/please | sh

<small>
Supported platforms are: Linux (64 or 32 bits) and OS X (64 bits only).
Windows users need to download [here](https://github.com/cloudfoundry/cli#downloads)
the original `cf` tool and rename it `gk`.
</small>

Once the installation is done, you are ready to log in to Gstack and push
containers or applications online.


## Log in to Gstack

First select the API endpoint for Gstack:

    gk api https://api.gstack.me

Then just login with `gk login`, and the tool will ask you your credential
like this:

    $ gk login
    API endpoint: https://api.gstack.me

    Email> benjamin

    Password> ********
    Authenticating...
    OK

Easy as that.

After you first login, you are operating in the context of a Gstack
“Organization”. A Gstack operator has created it for you, so you don't need to
bother.

Moreover, the applications you push will belong to a space. The Gstack
operator has created two spaces named `staging` and `production`. For a first
try, you'll need to target the `staging` space:

    gk target -s staging

And you are ready.


## Push your first Docker container

1. **Push** a sample container. To avoid name conflicts, pick a random number
   between 1 and 100, like `42` and append it to the name of your app in the
   following command:

        gk push my-first-gstack-app-42 -o cloudfoundry/lattice-app -m 4M -k 10M

   <small>
   (The `cloudfoundry/lattice-app` sample container is available
   [on Docker Hub](https://hub.docker.com/r/cloudfoundry/lattice-app/).)
   </small>

2. **Browse** your app on the web:

        open http://my-first-gstack-app-42.gstack.me

3. Once you're done, **delete** your test application and its route.

        gk delete my-first-gstack-app-42
        gk delete-route -f gstack.me -n my-first-gstack-app-42


## Push your first application

When you push a public container from Docker Hub, you get it online, but
as-is. What about pushing some applicaiton code that we have customized
ourselves?

In this process we don't need to first create a public Docker Hub container.
Instead, our application will be directly sent from our computer to Gstack,
and deployed there within a private container.

1. **Grab** a sample application

        git clone https://github.com/cloudfoundry-samples/lattice-app.git gstack-gsg
        cd gstack-gsg

2. **Customize** your application. Edit the `handlers/hello.go` file in your
   favorite text editor, and change the content of the `<div class="hello">`.
   For example, if your name is “Benjamin”, you can put this:

          <div class="hello">
              Benjamin here! Gstack Rocks!
          </div>

3. **Push** it to Gstack. To avoid name conflicts, pick a random number
   between 1 and 100, like `42`, and append it to the name of your app in the
   following command:

        gk push my-first-gstack-app-42 -b go_buildpack -m 4M -k 10M

   <small>
   (If you wonder what the above settings mean, try `gk push --help`.)
   </small>


4. **Browse** your app on the web!

        open http://my-first-gstack-app-42.gstack.me

   From there, you can try editing `handlers/hello.go` again, and `gk push`
   the result to update your app.

5. Once you're done, **delete** your test application and its route.

        gk delete my-first-gstack-app-42
        gk delete-route -f gstack.me -n my-first-gstack-app-42


## Go further

Now that you have pushed your first container and application, you might be
interested in [writing your own application](../create-application)!

In this guide, you may have noticed that you accessed your app with an
non-encrypted `http://` scheme. That's because Gstack creates “HTTP routes”
by default. Go read our [HTTPS Guide](../https-routes) to implement an
encrypted “HTTPS route”.

In this guide our sample application doesn't use any database. See our
[Services Guide](../plugging-services) to learn how to plug a database into
your app, and how the setup shall find its way into your `manifest.yml`
deployment descriptor.
