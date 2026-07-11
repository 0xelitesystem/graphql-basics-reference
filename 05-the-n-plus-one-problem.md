# The N plus one problem

The N plus one problem is when resolving a list of N items triggers one extra database query per item, so a single GraphQL query quietly turns into N plus 1 queries. It is the most common performance trap in GraphQL, and it comes straight from the field-by-field, item-by-item way resolvers run.

## How it happens

Consider a query for posts with each post's author:

```
query {
  posts {
    title
    author { name }
  }
}
```

The `posts` resolver runs one query and returns, say, 50 posts. Then the `author` resolver runs once per post, each firing its own `findUser` query. That is 1 query for the posts plus 50 queries for the authors: 51 total. Add another nested field and it multiplies again. Nothing looks wrong in the code; the cost is hidden in the tree walk.

## The fix: batching

The standard cure is to batch the per-item lookups into one. Instead of fetching each author individually, you collect all the author ids the resolvers ask for during a single tick, then issue one query like `findUsers([1, 2, 3, ...])`. The library most associated with this is DataLoader, which sits in the request context, coalesces the individual `load(id)` calls into a single batched call, and caches results within the request so the same id is never fetched twice.

```
context.userLoader.load(post.authorId);
```

Each resolver still asks for one author, but under the hood all those asks become one query.

## Caching within a request

DataLoader also deduplicates. If ten posts share the same author, the loader fetches that author once, not ten times. This per-request cache is separate from any long-lived cache and exists just to flatten the resolver tree's redundant lookups.

## Why it matters

Left unaddressed, N plus one turns a clean-looking query into a database storm under load. Because GraphQL lets clients request deeply nested data, the multiplication can be severe, so batching with a loader is considered standard practice rather than an optimization you add later.
