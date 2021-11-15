---
title: Current Location
date: Last Modified 
permalink: /current-location/index.html
eleventyNavigation:
  key: Current Location
  order: 11
  parent: Navigation
---
If you want to know the current location, use the `GoRouter.location` property.
If you'd like to know when the current location changes, either because of
manual navigation or a deep link or a pop due to the user pushing the Back
button, the `GoRouter` is a
[`ChangeNotifier`](https://api.flutter.dev/flutter/foundation/ChangeNotifier-class.html),
which means that you can call `addListener` to be notified when the location
changes, either manually or via Flutter's builder widget for `ChangeNotifier`
objects, the non-intuitively named
[`AnimatedBuilder`](https://stackoverflow.com/a/67016227):

```dart
class RouterLocationView extends StatelessWidget {
  const RouterLocationView({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    final router = GoRouter.of(context);
    return AnimatedBuilder(
      animation: router,
      builder: (context, child) => Text(router.location),
    );
  }
}
```

Or, if you're using [the provider package](https://pub.dev/packages/provider),
it comes with built-in support for re-building a `Widget` when a
`ChangeNotifier` changes with a type that is much more clearly suited for the
purpose.