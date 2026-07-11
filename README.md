# GraphQL Basics

A plain-language reference on GraphQL: a query language for APIs where the client asks for exactly the fields it wants from a single endpoint. It covers the schema and type system, the difference between queries, mutations, and subscriptions, how resolvers produce the data, how the single-endpoint model differs from REST, the N+1 problem that bites most teams, and an honest take on when GraphQL is worth it.

## The core idea

GraphQL is a query language and runtime for APIs in which the client sends a query describing the exact shape of data it needs, and the server returns just that shape from a single endpoint. Instead of many REST URLs each returning a fixed payload, there is one endpoint and a typed schema that defines everything askable, so the client can request several related resources in one round trip and get back no more and no less than it asked for. This flexibility is the appeal, and it is also the source of the operational challenges (caching, N+1 queries, cost control) that GraphQL asks you to manage.

## What is inside

- `01-schema-and-types.md` the typed schema that defines what can be asked.
- `02-queries-mutations-subscriptions.md` the three operation types and what each is for.
- `03-resolvers.md` the functions that actually fetch each field.
- `04-graphql-vs-rest.md` the single endpoint model against many REST URLs.
- `05-the-n-plus-one-problem.md` why naive resolvers explode into many queries, and how to fix it.
- `06-when-graphql-is-worth-it.md` where it pays off and where it does not.

## The stance

This reference treats GraphQL as a strong fit for clients that need flexible, precise access to a graph of related data, and as unnecessary overhead for simple or single-consumer APIs where REST is less to operate. It is candid about the costs GraphQL shifts onto the server, because those costs, not the query syntax, decide whether it was the right call.

## License

MIT. Copyright 0xelitesystem 2026.
