# Resolvers

A resolver is a function on the server that produces the value for a single field in the schema. When a query arrives, the GraphQL engine walks the requested fields and calls the matching resolver for each one, so the shape of the query drives which functions run. This is where the schema connects to your actual data, whether that is a database, another API, or a computed value.

## The resolver signature

A resolver typically receives four arguments: the parent value, the field arguments, a shared context, and info about the query. A common form looks like this:

```
const resolvers = {
  Query: {
    user: (parent, args, context) => {
      return context.db.findUser(args.id);
    },
  },
  User: {
    posts: (parent, args, context) => {
      return context.db.findPostsByAuthor(parent.id);
    },
  },
};
```

The `parent` for `User.posts` is the user object returned one level up, which is how nested fields get the data they need.

## Resolving is a tree walk

GraphQL resolves a query field by field, top down. It calls `Query.user`, takes that result, and for each requested sub-field either reads it straight off the object or calls a more specific resolver. If a field is a list, the child resolvers run once per item. This per-field, per-item execution is powerful, and it is also the root of the N plus one problem covered later.

## Context and defaults

The `context` object is created once per request and shared with every resolver, which is where you put the database connection, the authenticated user, and loaders. If you do not write a resolver for a field, GraphQL uses a default resolver that simply reads the property of the same name off the parent object, so you only write resolvers for fields that need real work.

## Keeping resolvers thin

Good resolvers stay small and delegate to a data layer rather than embedding queries and business logic inline. That keeps them testable and makes it easier to add batching and caching, which you will need once the naive one-query-per-field approach starts multiplying calls.
