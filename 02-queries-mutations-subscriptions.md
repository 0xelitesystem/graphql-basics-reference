# Queries, mutations, and subscriptions

GraphQL has three operation types: queries read data, mutations change data, and subscriptions stream live updates. They share the same field-selection syntax, and the difference is intent: a query is meant to have no side effects, a mutation is expected to write, and a subscription keeps a channel open for pushed events.

## Queries

A query asks for fields, and the response mirrors the shape of the request exactly:

```
query {
  user(id: "1") {
    name
    posts {
      title
    }
  }
}
```

You get back a JSON object with `name` and a `posts` list of `title` values, and nothing you did not ask for. Queries can take arguments and can be given names and variables for reuse.

## Mutations

A mutation changes server state and then returns whatever fields you request about the result, so you can read back the updated data in the same round trip:

```
mutation {
  createPost(title: "Hello", authorId: "1") {
    id
    title
  }
}
```

By convention writes go through mutations, and while the server does not force queries to be read-only, treating them that way keeps caching and reasoning sane. Multiple mutations in one request run in sequence, one after another, rather than in parallel.

## Subscriptions

A subscription opens a long-lived connection, usually over WebSockets, and the server pushes a message every time the subscribed event occurs:

```
subscription {
  postAdded {
    id
    title
  }
}
```

This is how you build live features like notifications or a feed that updates itself. Unlike queries and mutations, which are one response each, a subscription delivers many messages over time until the client unsubscribes.

## Variables

Rather than hardcoding values into the query string, pass them as typed variables, which is safer and lets the server cache the parsed query:

```
query GetUser($id: ID!) {
  user(id: $id) { name }
}
```

The variable values are sent alongside the query as a separate JSON object.
