---
layout: default
language: en
title: Gstack Documentation
tagline: <em>“Understanding is critical for a developer”</em> – B. Gandon
description:
---
### Welcome to Gstack!

Here at Gstack, we spend a great deal of effort in making sure that you'll
feel comfortable in using our platform.

If you spot any mistakes, missing details, or have improvements to submit,
don't hesitate to submit a PR on Github! Every page has a link to its Markdown
source on Github, to ease your finding the releavant files to update.


### For the newcomers

 - [Getting started with Gstack](./getting-started) – Where we tell you the
   basic steps to follow before entering the Gstack world.

 - [Your application in the Gstack context](./app-and-context) – Learn the
   mysteries of the underlying stack on which your applicaiton is going to
   run. You definitely want to read this.

 - [Database, Messaging… Plug services into you app!](./plugging-services) –
   What would be you application without persistence? How would you do
   micro-services without messaging? Gstack brings you all that very easily.

 - [HTTPS routes](./https-routes) – Unencrypted HTTP is dead. You definitely want
   to run HTTP<u>S</u>.


### For experienced buddies

 - [Continuous Integration](./continuous-integration) – Who can claim
   developing real applications with no automated tests and no team? Even if
   developing alone, you definitely want a Continuous Integration to run you
   tests automatically.

 - [Managing environments](./environments) – Most real-world apps are not a
   matter of a single production hosting, but a real development pipeline: a
   CI instance for your automated tests, a Demo for your Product Manager, an
   I18n instance for your translators, a Pre-Prod to test deployments in real
   conditions. Here we'll tell you how to implement such a pipeline on top of
   Gstack.

 - [Configuration Management](./config-management) – Once you have staging
   environments, they'll require dedicated configurations and you'll need to
   manage those configurations in a consistent way. This chapter will present
   you how you can solve this challenge.

 - [Very specific database or service? Bring it on!](./user-provided-services)
   – Gstack might not offer all the very specific services that you might want
   to plug into you application. Neo4j, edgy PostgreSQL version, Cassandra,
   name it. But Gstack gives you the opportunity to deploy them as a dedicated
   instance for your apps.

 - [Non-HTTP(S) apps? Need a TCP port? Come here!](./tcp-routes) – In the
   growing world of Internet-of-Things (IoT), your app might need to provide
   IoT devices with TCP services that are not based on HTTP. Let's dive into
   how to do that with Gstack.
