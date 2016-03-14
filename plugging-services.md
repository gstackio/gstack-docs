---
layout: default
language: en
title: Plug services into your apps
tagline: <em>“Who writes apps with no database out there? Who?”</em> <span class="nowrap">– B. Gandon</span>
description:
---

## Available services

To list the services that can be plugged into your application, run:

    $ gk marketplace

    service    plans                    description
    mysql      100mb-20cnx, 1gb-40cnx   MySQL databases, powered by MariaDB cluster
    postgres   shared, shared-nr        Reliable PostgrSQL Service

These are the standard services that Gstack can provide. If you need custom
services, you can push Docker containers and have your application use those.


## Plug a database, like MySQL

### Available plans

In this section, we'll take the example of a MySQL database. Let's detail the
available plans:

    $ gk marketplace -s mysql

There are two basic plans.

    service plan   description             free or paid
    100mb-20cnx    Small MySQL Database    free
    1gb-40cnx      Medium MySQL Database   free

The first is for small databases, below 100 MB of data. It allows 20
concurrent connections to each created database.

The second one allows larger databases up to 1 GB, and allows 40 concurrent
connections to it.

Both are powered by a MariaDB cluster that ensures good resiliency and
durability.


### Plug it to your app

It's a 2-steps process.

1. Create the database
2. Bind the database to an application


#### Create a database

To create the database, you specify the service name (i.e. the database
technology here), the service plan (which is the capacity required here), and
you give a name to this database.

For example, let's say you need a 100 MB MySQL database in your staging
environment for your application called `jelly-beans`. Then we advise you to
call it `mysql_db/jelly_beans_staging`. And you would type:

    gk create-service  mysql  100mb-20cnx  mysql_db/jelly-beans_staging

Your database is ready for being used by an application!


#### Bind the database

The recommended way of doing is to create a `manifest.yml` file, specify the
database service name in there, and finaly run `gk push`.

For the sample application of the [Getting Started](../getting-started) guide,
you would write such a following manifest. Use your favorite text editor and
create this `manifest.yml` file at the root of your application directory:

```yaml
---
applications:
  - name: my-first-gstack-app-42
    buildpack: go_buildpack
    memory: 4M
    disk_quota: 10M
    services:
      - mysql_db/jelly_beans_staging
```

Here you'll notice that we specified as
[YAML](https://en.wikipedia.org/wiki/YAML) properties the exact same arguments
as we did on the command line: `-b go_buildpack`, `-m 4M` and `-k 10M`. The
new part is the `services:` list, which only item specifies the database we've
just created above.

After this, you can run `gk push` without any arguments, because `gk` will
find all the necessary information in this `manifest.yml`.

#### Different databases for different environments

When you use a `staging` space and a `production`space to model different
environments, you create two manifest files instead of one. They will be
called `manifest_staging.yml` and `manifest_production.yml`. Each one of them
will specify a different database name. For example, the
`manifest_production.yml` would look like this:

```yaml
---
applications:
  - name: ...
    ...
    services:
      - mysql_db/jelly_beans_production
```

And you would use one of them running `gk push -f manifest_staging.yml` or
`gk push -f manifest_production.yml`

> When the application is already started, an alternate way of binding
> the database is to run the `gk bind-service` command and then do a
> `gk restage` for the app to become aware of the database. We won't
> cover this option here, and let it as an exercise to the reader.


## PostgreSQL databases

In this section, we'll focus on some specificities of the PostgreSQL service,
as provided by Gstack. For more info on how to create a database and bind it
to an application, see the “[Plug a database](#plug-a-database)” section
above.

For a start, let's detail the available PostgreSQL plans:

    $ gk marketplace -s postgres

    service plan   description                                                     free or paid
    shared         A Reliable PostgreSQL database on a shared server.              free
    shared-nr      A PostgreSQL database with no replication on a shared server.   free

The `shared` plan refers to PostgeSQL databases on a shared cluster. These are
powered by [BDR](http://bdr-project.org/docs/stable/), a “Bi-Directional
Replication” technology for building resilient PostgreSQL clusters.

Unfortunately BDR has some limitations, especially with schema changes. See
the [Prohibited DDL statements](http://bdr-project.org/docs/stable/ddl-replication-statements.html#DDL-REPLICATION-PROHIBITED-COMMANDS)
section of the BDR documentation to learn more about those.

That's why the `postgres` service also provides a `shared-nr` plan. This
second option is a traditional PostgreSQL 9.4 database, with no replication,
thus no limitation. The tradeoff is that it has more limited data durability
for the user due to the lack of replication. For Gstack operators, it has less
scaling flexibility due to the lack of clustering.


## Further readings

Now that you are confident in how to fuel your application with databases, why
not try to [Build your own](../create-application)?
