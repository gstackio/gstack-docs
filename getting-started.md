---
layout: default
language: en
title: Getting started with Gstack
tagline: <em>“Every project should onboard newcomers in a snap”</em> <span class="nowrap">– B. Gandon</span>
description:
---
<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>

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

    Email> benjamin@exmple.org

    Password> ********
    Authenticating...
    OK

Easy as that.

After you first login, you are operating in the context of a Gstack
“Organization”. A Gstack operator has created it for you, based on your family
name. Anyway, you don't need to bother.

Moreover, the applications you push will belong to a “Space”, inside your
“Organization”. The Gstack operator has created a default space named
`staging` for you to start using Gstack. Right now, you don't need to bother.
“Organization” and “Space” are just organizational boxes that tell _where_
your applications will be on Gstack.

So, you are ready.


## Play around with a Docker container

The whole point in this example is to push an application on Gstack, and see
that you can access it immediately with your web brower.

This requires a domain name to acces your app. Good news, a Gstack operator
has created a `gstack.me` subdomain for you only, named after your family
name. For example, if your family name is `gandon`, your own subdomain will be
`gandon.gstack.me`. You can see that with the following command:

        $ gk domains
        ...
        gandon.gstack.me   owned

1. Select your own subdomain and put it in a variable for later use (and
   simplicity of this tutorial):

        owned_domain=$(LANG=en gk domains | awk '/owned/{print $1}')

2. **Push** a sample container.

        gk push wow-cool -o gstack/sample-app -m 8M -k 16M -d "$owned_domain"

   <small>
   (The `gstack/sample-app` sample container is available
   [on Docker Hub](https://hub.docker.com/r/gstack/sample-app/).)
   </small>

3. **Browse** your app on the web.

        open "http://wow-cool.$owned_domain"

4. **Scale up** your app to 3 instances.

        gk scale wow-cool -i 3

5. **Refresh** your browser several time to see the app index changing. This
   means that web requests are properly load-balanced across the 3 instances
   of your app.

6. Once you're done, **clean up** your resources, deleting the test app and
   its route. (Once again, replace `gandon` by your actual family name.)

        gk delete wow-cool
        gk delete-route "$owned_domain" -n wow-cool


## Push your own source code

When you push a public container from Docker Hub, you get it online, but
as-is. What about pushing some applicaiton code that we have customized
ourselves?

In this process we don't need to first create a public Docker Hub container.
Instead, our application will be directly sent from our computer to Gstack,
and deployed there within a private container.

1. **Grab** a sample application

        git clone https://github.com/gstackio/sample-app.git gstack-gsg
        cd gstack-gsg

2. **Customize** your application. Edit the `handlers/hello.go` file in your
   favorite text editor, and change the content of the `<div class="hello">`.
   For example, if your name is “Benjamin”, you can put this:

          <div class="hello">
              Benjamin here! Gstack Rocks!
          </div>

3. **Push** it to Gstack.

        owned_domain=$(LANG=en gk domains | awk '/owned/{print $1}')

        gk push i-am -d "$owned_domain"

   Here you see that your source code is first built, and then deployed on
   Gstack.

   You'll notice that we create a shell variable named `owned_domain`. That's
   just to automate the work of selecting your own subdomain when deploying
   the app. This ensures your app is accessed with a URL that is unique.

4. **Browse** your app on the web!

        open "http://i-am.$owned_domain"

   From there, you can try editing `handlers/hello.go` again, and `gk push`
   the result to update your app.

5. Once you're done, **delete** your the created resources, i.e. the test
   application and its route.

        gk delete i-am
        gk delete-route "$owned_domain" -n i-am


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

<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>
