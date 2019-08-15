---
title: "What Is Graphql"
date: 2019-08-14T10:13:21+03:00
draft: false
tags:
    - introduction
    - graphql
    - api
---

> GraphQL is a syntax that describes how to ask for data, and is generally used to load data from a server to a client. GraphQL has three main characteristics:

+ It lets the client specify exactly what data it needs.
+ It makes it easier to aggregate data from multiple sources.
+ It uses a type system to describe data.

With GraphQL, the user is able to make a single call to fetch the required information rather than to construct several REST requests to fetch the same.

**N/B**
GraphQL and REST are two different things -- GraphQL is a language and a technology, while REST is an architecture pattern, which means that even as teams increasingly adopt GraphQL, it does not mean the end of the road for REST.

GraphQL has proven itself as a solution to aggregate data from multiple sources, specify data, and describe data. The real secret is that GraphQL ensures that the developer and application only loads the relevant and absolute necessary data, even if it's from multiple sources.

### What is a GraphQL Server?

The GraphQL execution engine is what is responsible for processing a GraphQL query and returning a JSON response. All GraphQL servers are made up of two core components that define the structure and behavior of the execution engine: a schema and resolvers, respectively.

A GraphQL schema is a custom typed language that exposes which queries are both permitted (valid) and handled by a GraphQL server implementation. The schema for our user example query above might look like:

```js
type User {
        name: String
        email: String
        phoneNumber: String
        address: String
}

type Query {
        user: User
}
```

This schema defines a user query that returns a user. Clients can request any of the fields on a user via the user query, and the GraphQL server will return only those fields in its response. By using the strongly typed schema, a GraphQL server can validate incoming queries to ensure they are valid based on the defined schema.

Once a query is determined to be valid, it is processed by a GraphQL server by resolvers. A resolver function backs each field of each GraphQL type. An example resolver for our user query might look like:

```js
Query: {
  user(obj, args, context, info) {
    return context.db.loadUserById(args.id).then(
      userData => new User(userData)
    )
  }
}
```

While the above example is in JavaScript, GraphQL servers can be written in any number of languages. This is due to the fact that GraphQL is also a specification!

A GraphQL server should be able to:

+ Receive requests following the GraphQL format, for example:

```json
{ "query": "query { allLinks { url } }" }
```

+ Connect to any necessary databases or services responsible for storing/fetching the actual data.

+ Return a GraphQL response with the requested data, such as this:

```json
{ "data": { "allLinks": { "url": "http://graphql.org/" } } }
```

+ Validate incoming requests against the schema definition and the supported format. For example, if a query is made with an unknown field, the response should be something like:

```json
{
  "errors": [{
    "message": "Cannot query field \"unknown\" on type \"Link\"."
  }]
}
```

More can be found in the [official specification](https://facebook.github.io/graphql/)

### References

1. [What is GraphQL?](https://opensource.com/article/19/6/what-is-graphql)

2. 