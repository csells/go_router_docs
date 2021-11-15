---
title: Initialization
date: Last Modified 
permalink: /initialization/index.html
eleventyNavigation:
  key: Initialization
  order: 8
  parent: Declarative Routing
---
With just a list of routes and an error page builder function, you can create an
instance of a `GoRouter`, which itself provides the objects you need to call the
`MaterialApp.router` constructor:

```dart
class App extends StatelessWidget {
  App({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) => MaterialApp.router(
        routeInformationParser: _router.routeInformationParser,
        routerDelegate: _router.routerDelegate,
      );

  final _router = GoRouter(routes: ..., errorPageBuilder: ...);
}
```

With the router in place, your app can now navigate between pages.