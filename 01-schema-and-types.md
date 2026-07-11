# Schema and types

A GraphQL schema is a typed contract that defines every type of data and every operation a client can ask for. It is written in the Schema Definition Language (SDL), and because it is strongly typed, both the server and the client know exactly what fields exist and what shape they take before any query runs.

## Object types and fields

The core building block is the object type, a named collection of fields, each with its own type:

```
type User {
  id: ID!
  name: String!
  email: String
  posts: [Post!]!
}

type Post {
  id: ID!
  title: String!
  author: User!
}
```

A `!` means the field is non-null (always present). Square brackets mean a list, so `[Post!]!` is a non-null list of non-null posts. Notice fields can point at other types (`author: User`), which is how the schema forms a graph of connected data.

## Scalars and enums

The built-in scalar types are `Int`, `Float`, `String`, `Boolean`, and `ID`. You can define enums to constrain a field to a fixed set of values:

```
enum Role {
  ADMIN
  MEMBER
  GUEST
}
```

You can also define custom scalars (like `DateTime`) when the built-ins are not enough.

## The root types

Three special types are the entry points into the schema. `Query` holds the readable fields, `Mutation` holds the operations that change data, and `Subscription` holds streams of events:

```
type Query {
  user(id: ID!): User
  posts: [Post!]!
}
```

A client can only ask for what these root types expose, following the graph from there.

## Why the schema matters

Because everything is typed and declared up front, the server can validate a query before executing it, tools can autocomplete and check queries as you write them, and the schema doubles as always-accurate documentation. The schema is the single source of truth that both sides agree on.
