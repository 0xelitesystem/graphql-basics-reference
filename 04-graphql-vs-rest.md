# GraphQL vs REST

The defining difference is that REST exposes many endpoints each returning a fixed payload, while GraphQL exposes one endpoint where the client specifies the exact fields it wants. That single shift explains most of the tradeoffs between the two styles.

## One endpoint, client-shaped responses

In REST you call `/users/1`, then maybe `/users/1/posts`, then `/posts/5/comments`, stitching several requests together and often receiving fields you do not use. In GraphQL you send one query to `/graphql` and describe the whole tree you need:

```
query {
  user(id: "1") {
    name
    posts { title comments { text } }
  }
}
```

One round trip returns exactly that. This directly targets two REST pain points: overfetching (getting fields you did not want) and underfetching (having to make follow-up calls for related data).

## What REST gives you for free

REST rides on HTTP conventions, and that is a real advantage. HTTP status codes, caching by URL in browsers, CDNs, and proxies, and simple debugging all come built in. Because a GraphQL request is usually a POST to one URL, standard HTTP caching does not apply, so caching moves into the client library and the server, which is more work.

## Trade-offs at a glance

GraphQL wins on flexibility, precise data fetching, a strongly typed self-documenting schema, and fewer round trips for complex screens. REST wins on simplicity, HTTP-native caching, easy monitoring per endpoint, and a lower operational burden. GraphQL also concentrates cost control on the server, since a single query can ask for an enormous graph, something REST spreads across many separate, individually limited endpoints.

## Not mutually exclusive

The two are not a strict either-or. Many systems expose REST for simple public access and GraphQL for rich client apps, or put a GraphQL layer in front of existing REST services. The right question is which style fits a given consumer, not which is universally better.
