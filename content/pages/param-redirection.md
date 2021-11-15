---
title: Parameterized Redirection
date: Last Modified 
permalink: /param-redirection/index.html
eleventyNavigation:
  key: Parameterized Redirection
  order: 20
  parent: Redirection
---
In some cases, a path is parameterized, and you'd like to redirect with those
parameters in mind. You can do that with the `params` argument to the `state`
object passed to the `redirect` function:

```dart
GoRoute(
  path: '/author/:authorId',
  redirect: (state) => '/authors/${state.params['authorId']}',
),
```