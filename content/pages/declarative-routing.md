---
title: Declarative Routing
date: Last Modified 
permalink: /declarative-routing/index.html
eleventyNavigation:
  key: Declarative Routing
  order: 5
---
The go_router is governed by a set of routes which you specify as part of the
`GoRouter` constructor:

```dart
class App extends StatelessWidget {
  ...
  final _router = GoRouter(
    routes: [
      GoRoute(
        path: '/',
        pageBuilder: (context, state) => MaterialPage<void>(
          key: state.pageKey,
          child: const Page1Page(),
        ),
      ),
      GoRoute(
        path: '/page2',
        pageBuilder: (context, state) => MaterialPage<void>(
          key: state.pageKey,
          child: const Page2Page(),
        ),
      ),
    ],
  ...
  );
}
```

In this case, we've defined two routes. Each route `path` will be matched
against the location to which the user is navigating. Only a single path will be
matched, specifically the one that matches the entire location (and so it
doesn't matter in which order you list your routes). The `path` will be matched
in a case-insensitive way, although the case for [parameters](/parameters) will
be preserved.