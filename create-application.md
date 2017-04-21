---
layout: default
language: en
title: Write your own application
tagline: <em>“Developers love to express their creativity”</em> <span class="nowrap">– B. Gandon</span>
description:
---
<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>

## Overview

This is a 3-steps process.

1. Write an application with whatever technology you like.
2. Write a deployment `manifest.yml` file, containing just a few
   [YAML](https://en.wikipedia.org/wiki/YAML) properties.
3. Comply to some Cloud Foundry conventions, depending on you app technology
   (Ruby, Java, etc).

Ready? Let's go!


## Write an application

Go follow our detailed step-by-step tutorials, depending on which language you
are familiar with. Those will guide you in writing web applications that
follow state-of-the-arts best practice.

 - [Go](../create-application/golang)
 - [Java](../create-application/java)
 - [Node.js](../create-application/node)
 - [PHP](../create-application/php)
 - [Python](../create-application/python)
 - [Ruby](../create-application/ruby)


### Write your `manifest.yml`

*More to come in this section. PRs are welcome!*

In the meanwhile, go read the
“[Deploying with Application Manifests](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html)”
section of the stock Cloud Foundry docs.


### Comply with Cloud Foundry conventions

*More to come in this section. PRs are welcome!*

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


## Ready to `gk push`?

That's it, you're ready.

If you wonder how to use `gk push`, just type `gk push --help`. We some more
hints in our [Command Line](../command-line) section.

You'll find more information in the
“[Develop and Manage Applications](https://docs.cloudfoundry.org/devguide/)”
section of Cloud Foundry docs.


## Further readings

After you `gk push` your app, Gstack will create an “HTTP route”, so that you
can access it with your web browser. Read our [HTTPS Guide](../https-routes) to
implement an encrypted “HTTPS route”.

To plug a database into your app, see our [Services Guide](../plugging-services).
You'll learn how those services shall find their way into your
`manifest.yml` deployment descriptor.

<small>[← Back to Gstack Docs]({{ site.baseurl }})</small>
