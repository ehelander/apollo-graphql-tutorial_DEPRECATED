# Apollo tutorial

This is the fullstack app for the [Apollo tutorial](http://apollographql.com/docs/tutorial/introduction.html). 🚀

## File structure

The app is split out into two folders:
- `start`: Starting point for the tutorial
- `final`: Final version

From within the `start` and `final` directories, there are two folders (one for `server` and one for `client`).

## Installation

To run the app, run these commands in two separate terminal windows from the root:

```bash
cd final/server && npm i && npm start
```

and

```bash
cd final/client && npm i && npm start
```
# Eric's Notes

## Tutorial

### [0. Introduction](https://www.apollographql.com/docs/tutorial/introduction/)
- Set up your development environment
  - [Apollo Engine](https://engine.apollographql.com/)
    - https://engine.apollographql.com/account/gh.ehelander/services?loginAttempt=1
  - [Apollo DevTools for Chrome](https://chrome.google.com/webstore/detail/apollo-client-developer-t/jdkknkkbebbapilgoeccciglkfbmbnfm)
  - [Apollo VSCode](https://marketplace.visualstudio.com/items?itemName=apollographql.vscode-apollo)
- Clone repo: `git clone https://github.com/apollographql/fullstack-tutorial/`
### [1. Build a schema](https://www.apollographql.com/docs/tutorial/schema/)
#### [Set up Apollo Server](https://www.apollographql.com/docs/tutorial/schema/#set-up-apollo-server)
- Install project dependencies
  - `cd start/server && npm install`
#### [Write your graph's schema](https://www.apollographql.com/docs/tutorial/schema/#write-your-graphs-schema)
- **Recommendation**: Schema-First Development
  - Agree upon the schema before beginning any API development.
##### [Query type](https://www.apollographql.com/docs/tutorial/schema/#query-type)
- SDL: schema definition language (syntax similar to TypeScript)
- All types in GraphQL are nullable by default. Add `!` to indicate that a query will always return data.
##### [Object & scalar types](https://www.apollographql.com/docs/tutorial/schema/#object--scalar-types)
- Think of scalars as the leaves of the graph that all fields resolve to.
  - Can define [custom scalars](https://www.apollographql.com/docs/apollo-server/features/scalars-enums/) (such as Date).
- Any GraphQL fields can contain arguments, not just queries.
- [Schema reference](https://devhints.io/graphql#schema)
##### [Mutation type](https://www.apollographql.com/docs/tutorial/schema/#mutation-type)
- The `Mutation` type is the entry point into the graph for modifying data.
- **Recommendation**: Define a special response type to ensure a proper response is returned back to the client.
- **Best practice**: Return the data that you're updating in order for the Apollo Client cache to update automatically.
##### [Run your server](https://www.apollographql.com/docs/tutorial/schema/#run-your-server)
- `server.listen()`
- `npm start`
##### [Explore your schema](https://www.apollographql.com/docs/tutorial/schema/#explore-your-schema)
- [GraphQL Playground](https://www.apollographql.com/docs/apollo-server/features/graphql-playground/) provides the ability to introspect.
  - Introspection: Technique used to provide detailed information about a graph's schema.
  - The `SCHEMA` button provides quick access to the documentation of a GraphQL API.

### [2. Hook up your data sources](https://www.apollographql.com/docs/tutorial/data-source/)
- Apollo data source: A class that encapsulates all the data fetching logic, as well as caching and deduplication, for a particular service.
- **Best practice** for organizing code: Use Apollo data sources to hook up your services to your graph API
#### [Connect a REST API](https://www.apollographql.com/docs/tutorial/data-source/#connect-a-rest-api)
- [Space-X v2 REST API](https://github.com/r-spacex/SpaceX-API)
- `npm install apollo-datasource-rest --save`
  - Exposes `RESTDataSource` class (responsible for fetching data from a REST API)
    - To build a data source for a REST API, extend `RESTDataSource` and define `this.baseURL`
- Apollo `RESTDataSourcd` sets up an in-memory cache that caches responses from our REST resources with no additional setup: [partial query caching](https://blog.apollographql.com/easy-and-performant-graphql-over-rest-e02796993b2b)
##### [Write data fetching methods](https://www.apollographql.com/docs/tutorial/data-source/#write-data-fetching-methods)
- Add methods to the data source that correspond to the queries our graph API needs to fetch.
- Apollo REST data sources have helper methods that correpond to HTTP verbs (e.g., `GET` -> `this.get('item')`).
- **Recommendation**: Write a specific reducer to transform response into the shape our schema expects (enables decoupling the graph API from the business logic specific to the REST API).
#### [Connect a database](https://www.apollographql.com/docs/tutorial/data-source/#connect-a-database)
##### [Build a custom data source](https://www.apollographql.com/docs/tutorial/data-source/#build-a-custom-data-source)
- Apollo doesn't yet support a SQL data source; need to create a custom data source for the database by extending the generic Apollo data source class.
  - Core concepts for creating a custom data source
    - `initialize` method
      - Necessary for passing any configuration options to the class.
    - `this.context`
      - An object that's shared among every resolver of a GraphQL request.
    - caching
      - The generic data source does not have built-in caching. Build your own with [cache primitives](https://www.apollographql.com/docs/apollo-server/features/data-sources/#using-memcached-redis-as-a-cache-storage-backend)
#### [Add data sources to Apollo Server](https://www.apollographql.com/docs/tutorial/data-source/#add-data-sources-to-apollo-server)
- Add a data source: Create a `dataSources` property on `ApolloServer` that corresponds to a function that returns an object with your instantiated data sources.
- **Critical**: If you use `this.context` in your datasource, create a new instance in the `dataSources` function instead of sharing a single instance (since `initialize` may be called during the execution of asynchronous code for a specific user and replace `this.context` by the context of another user).
