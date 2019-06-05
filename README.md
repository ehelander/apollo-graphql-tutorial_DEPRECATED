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
#### [Query type](https://www.apollographql.com/docs/tutorial/schema/#query-type)
- SDL: schema definition language (syntax similar to TypeScript)
- All types in GraphQL are nullable by default. Add `!` to indicate that a query will always return data.
#### [Object & scalar types](https://www.apollographql.com/docs/tutorial/schema/#object--scalar-types)
- Think of scalars as the leaves of the graph that all fields resolve to.
  - Can define [custom scalars](https://www.apollographql.com/docs/apollo-server/features/scalars-enums/) (such as Date).
- Any GraphQL fields can contain arguments, not just queries.
- [Schema reference](https://devhints.io/graphql#schema)
#### [Mutation type](https://www.apollographql.com/docs/tutorial/schema/#mutation-type)
- The `Mutation` type is the entry point into the graph for modifying data.
- **Recommendation**: Define a special response type to ensure a proper response is returned back to the client.
- **Best practice**: Return the data that you're updating in order for the Apollo Client cache to update automatically.
#### [Run] your server(https://www.apollographql.com/docs/tutorial/schema/#run-your-server)
- `server.listen()`
- `npm start`
#### [Explore your schema](https://www.apollographql.com/docs/tutorial/schema/#explore-your-schema)
- [GraphQL Playground](https://www.apollographql.com/docs/apollo-server/features/graphql-playground/) provides the ability to introspect.
  - Introspection: Technique used to provide detailed information about a graph's schema.
  - The `SCHEMA` button provides quick access to the documentation of a GraphQL API.
