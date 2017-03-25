[![CircleCI](https://circleci.com/gh/cognitect-labs/vase.svg?style=svg&circle-token=21b84b7aea75483821d3852de6c5d9930e85720a)](https://circleci.com/gh/cognitect-labs/vase)

# Vase: Data-driven microservices

This system provides a data-driven and extensible way to describe and
run HTTP APIs that are backed by a durable database (Datomic).

### Vase should be considered beta technology

Vase has been used on real-world projects. Each new project teaches us
something more about what Vase can do. We're continuing to improve its
utility and usability. You may find some rough edges here and
there. When you do, raise an issue so we can make it better.

## Releases and Dependency Information

If you would like to use the latest developments in Vase, you will need to
clone this repository and install it locally:

```
git clone https://github.com/cognitect-labs/vase
cd vase
lein install
```

Stable versions are currently deployed to the Clojars repository.

[Leiningen](https://github.com/technomancy/leiningen) dependency information:

```clj
 [com.cognitect/pedestal.vase "0.9.0"]
```

[Maven](http://maven.apache.org/) dependency information:

```xml
<dependency>
  <groupId>com.cognitect</groupId>
  <artifactId>pedestal.vase</artifactId>
  <version>0.9.0</version>
</dependency>
```

## Before you get started

Vase is built on top of [Pedestal](http://pedestal.io/)
and [Datomic](http://www.datomic.com/). While you don't need to be a
Pedestal or Datomic expert to use Vase, a little introductory material
goes a long way. Newcomers to either will find these resources especially helpful.

### Pedestal

[Pedestal](http://pedestal.io/index#what-is-pedestal) is a collection
of libraries for building services and applications. Pedestal routes
wire requests to one or more Pedestal [interceptors](http://pedestal.io/reference/interceptors)
which handle request/response processing. As you'll find out later, Pedestal interceptors
are a crucial part of Vase so it is helpful to understand what they are and how they work.

### Datomic

Datomic is a database of [facts](http://docs.datomic.com/query.html#database-of-facts) and Vase uses it as its backend store. You will immediately be confronted by three Datomic concepts as you work with Vase: schema, query and transaction. Of the three, Datomic queries offer the most variety and, possibly, confusion. Datomic uses a declarative query language called Datomic Datalog for queries. [Learn Datalog Today](http://www.learndatalogtoday.org/) will
help you get up to speed with it.

The Datomic docs [site](http://docs.datomic.com/index.html) has in-depth resources covering [schema](http://docs.datomic.com/schema.html),
[query](http://docs.datomic.com/query.html) and [transactions](http://docs.datomic.com/transactions.html)
and more. These are good resources to dive into as you move to more advanced usages of Vase.

## Getting Started

By default, Vase uses an in-memory Datomic database, using the
[publicly available Datomic-free](https://clojars.org/com.datomic/datomic-free)
version located in Clojars.

Everything you need to get up and running with Vase is packaged in Vase itself.
Vase is completely self-contained -- You don't need to setup Datomic or create a MyDatomic account.

### Using the template

This repository also includes a Leiningen [template](./template) for
getting started.  Look at the [template's
README](./template/README.md) for local/developer setup, otherwise

`lein new vase my-service`

### Adding Vase to a Pedestal project

Vase turns API descriptions into Pedestal routes. The API descriptions
specify actions for routes, these are turned into interceptors in the
Pedestal routes. This is done by `vase/routes`.

`vase/routes` takes an API base URL and an API specification. The routes it
returns can be used directly by Pedestal like this:

```clj
(require '[io.pedestal.http.route.definition.table :as table])
(require '[com.cognitect.vase]))

(defn make-master-routes
  [spec]
  (table/table-routes
   {}
   (vase/routes "/api" spec)))
```

The routes that `vase/routes` returns can also be combined with
hand-written Pedestal routes by concatenating the input to
`table/table-routes`:

```clj
(require '[io.pedestal.http.route.definition.table :as table])
(require '[com.cognitect.vase]))

(defn make-master-routes
  [spec]
  (table/table-routes
   {}
   (concat
     [["/hello" :get [hello]]]
     (vase/routes "/api" spec))))
```


## Documentation

* [Adding Vase to an existing Pedestal service](./docs/adding_vase.md)
* [Building Your First API](./docs/your_first_api.md)
* [Design document](./docs/design.md)


## Contributing

Contributing guidelines for Vase are the same as
[Pedestal](https://github.com/pedestal/pedestal/blob/master/CONTRIBUTING.md).

For code contribution, you'll need a signed [Cognitect Contributor
Agreement](https://secure.echosign.com/public/hostedForm?formid=8JU33Z7A7JX84U).
For small changes and doc updates, no CA is required.

If you're proposing a significant change, please open an Issue to
discuss the design of the change before submitting a Pull Request.

## Support

Don't hesitate to reach out if you run into issues or have questions!
The Vase community can be found in the
the [pedestal-users](https://groups.google.com/d/forum/pedestal-users)
mailing list or
the [#pedestal](https://clojurians.slack.com/messages/pedestal/) slack
channel.

## Copyright

Copyright 2015-2017 Cognitect, Inc. All rights reserved.
