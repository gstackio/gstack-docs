---
layout: default
language: en
title: HTTPS routes for your apps
tagline: <em>“Nowadays, no apps should consider HTTP anymore”</em> <span class="nowrap">– B. Gandon</span>
description:
---
## It's already there, baby

There's actually nothing to do for `https://` on Gstack. It just works out of
the box.

No setup, no certificates fuss, nothing. You access your application with the
`https://` scheme and it just works.

There's a few thing to note about that, though.


## From your app point of view

Requests to `https://` pages are transparently proxied to your app by the
Gstack infrastructure. You application only has to answer `http://` requests.

If your application needs to differentiate whether it is access securely or
not, go have a look at the `X-Forwarded-Proto` HTTP header in the request that
you app receives.


## How to use my own domain?

You just have to point you DNS record to the Gstack platform. For this, create
a `CNAME` record that points to `prod.gstack.io.`.

And then you'll create the corresponding route in Gstack. Most of the time,
you'll just push your app with your own domain specified as its route <small>(like in
the [Getting Started](../getting-started) guide)</small>, and this will create the
expected route for you.


## Behind the scenes

We automatically generate [Let's Encrypt](https://letsencrypt.org/)
certificates when you first access your app with your web browser. This
process takes about 8 seconds to complete.

But when you bind your app to a `gstack.me` subdomain, be aware that that we
are subject to Let's Encrypt
[rate limits](https://letsencrypt.org/docs/rate-limits/)! If not already done,
**we urge you to consult these limits** and be very frugal when generating
certificates for new domain names!

That's why you should observe the following rule:


**_→ Please only start using HTTPS when you are sure you won't change the route
that your app is bound to._**


## Why do we speak about “Routes”?

When you push your app to Gstack, it is deployed and bound to a URL. Actually,
from the platform point of view, the URL is something that participates in
routing web requests through the infrastructure to your application.

Thus, web traffic is actually routed forth (for web requests) and back (for
web responses). That's why we speak about HTTPS “routes”.

If you are curious, you can list the routes created in your current Org and
current Space with the `gk routes` command. Once you create a route in a Space
of your Org, no other Space in any other Org can create the exact same route.
