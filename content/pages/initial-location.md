---
title: Initial Location
date: Last Modified 
permalink: /initial-location/index.html
eleventyNavigation:
  key: Initial Location
  order: 10
  parent: Navigation
---
If you'd like to set an initial location for routing, you can set the
`initialLocation` argument of the `GoRouter` constructor:

```dart
final _router = GoRouter(
  routes: ...,
  errorPageBuilder: ...,
  initialLocation: '/page2',
);
```

The value you provide to `initialLocation` will be ignored if your app is started
using [deep linking](#deep-linking).