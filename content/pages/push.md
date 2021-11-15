---
title: Push
date: Last Modified 
permalink: /push/index.html
eleventyNavigation:
  key: Push
  order: 12
  parent: Navigation
---
In addition to the `go` method, the go_router also provides a `push` method.
Both `go` and `push` can be used to build up a stack of pages, but in different
ways. The `go` method does this by turning a single location into any number of
pages in a stack using [Sub-routes](#sub-routes).

The `push` method is used to push a single page onto the stack of existing
pages, which means that you can build up the stack programmatically instead of
declaratively. When the `push` method matches an entire stack via sub-routes, it
will take the top-most page from the stack and push that page onto the stack.

You can also push a [named route](#named-routes), discussed below.
