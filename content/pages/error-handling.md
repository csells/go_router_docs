---
title: Error Handling
date: Last Modified 
permalink: /error-handling/index.html
eleventyNavigation:
  key: Error Handling
  order: 7
  parent: Declarative Routing
---
In addition to the list of routes, the go_router needs an `errorPageBuilder`
function in case no page is found, more than one page is found or if any of the
page builder functions throws an exception, e.g.

```dart
class App extends StatelessWidget {
  ...
  final _router = GoRouter(
    ...
    errorPageBuilder: (context, state) => MaterialPage<void>(
      key: state.pageKey,
      child: ErrorPage(state.error),
    ),
  );
}
```

The `GoRouterState` object contains the location that caused the exception and
the `Exception` that was thrown attempting to navigate to that route.