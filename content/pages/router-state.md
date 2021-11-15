---
title: Router State
date: Last Modified 
permalink: /router-state/index.html
eleventyNavigation:
  key: Router State
  order: 6
  parent: Declarative Routing
---
A `GoRoute` also contains a `pageBuilder` function which is called to create the
page when a path is matched. The builder function is passed a `state` object,
which is an instance of the `GoRouterState` class that contains some useful
information:

| `GoRouterState` property | description | example 1 | example 2 |
| ------------------------ | ----------- | ------- | ------- |
| `location` | location of the full route, including query params | `/login?from=/family/f2` | `/family/f2/person/p1`|
| `subloc` | location of this sub-route w/o query params | `/login` | `/family/f2` |
| `name` | the `GoRoute` name | `login` | `family` |
| `path` | the `GoRoute` path | `/login` | `family/:fid` |
| `fullpath` | full path to this sub-route | `/login` | `/family/:fid` |
| `params` | params extracted from the location | `{}` | `{'fid': 'f2'}` |
| `queryParams` | optional params from the end of the location | `{'from': '/family/f1'}` | `{}` |
| `extra` | optional object param | `null` | `null` |
| `error` | `Exception` associated with this sub-route, if any | `Exception('404')` | ... |
| `pageKey` | unique key for this sub-route | `ValueKey('/login')` | `ValueKey('/family/:fid')` |

You can read more about [sub-locations/sub-routes](/sub-routes) and
[parameterized routes](/parameters) below but the example code above uses the
`pageKey` property as most of the example code does. The `pageKey` is used to
create a unique key for the `MaterialPage` or `CupertinoPage` based on the
current path for that page in the [stack of pages](/sub-routes), so it will
uniquely identify the page w/o having to hardcode a key or come up with one
yourself.

Not all of the state parameters will be set every time. In general, the state is
a superset of the potential current state of a `GoRouter` instance. For example,
the `error` parameter will only be set of there's an error, the `params` won't
be set during top-level redirection because there's no `path` to match yet, etc.
