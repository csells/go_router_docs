---
title: Route-level Redirection
date: Last Modified 
permalink: /route-level-redirection/index.html
eleventyNavigation:
  key: Route-level Redirection
  order: 19
  parent: Redirection
---
The top-level redirect handler passed to the `GoRouter` constructor is handy
when you want a single function to be called whenever there's a new navigation
event and to make some decisions based on the app's current state. However, in
the case that you'd like to make a redirection decision for a specific route (or
sub-route), you can do so by passing a `redirect` function to the `GoRoute`
constructor:

```dart
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      redirect: (_) => '/family/${Families.data[0].id}',
    ),
    GoRoute(
      path: '/family/:fid',
      pageBuilder: ...,
  ],
  errorPageBuilder: ...,
);
```

In this case, when the user navigates to `/`, the `redirect` function will be
called to redirect to the first family's page. Redirection will only occur on
the last sub-route matched, so you can't have to worry about redirecting in the
middle of a location being parsed when you're already on your way to another
page anyway.