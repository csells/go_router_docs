---
title: Navigation
date: Last Modified 
permalink: /navigation/index.html
eleventyNavigation:
  key: Navigation
  order: 9
---
To navigate between pages, use the `GoRouter.go` method:

```dart
// navigate using the GoRouter
onTap: () => GoRouter.of(context).go('/page2')
```

The go_router also provides a simplified means of navigation using Dart
extension methods:

```dart
// more easily navigate using the GoRouter
onTap: () => context.go('/page2')
```

The simplified version maps directly to the more fully-specified version, so you
can use either. If you're curious, the ability to just call `context.go(...)`
and have magic happen is where the name of the go_router came from.

If you'd like to navigate via [the `Link`
widget](https://pub.dev/documentation/url_launcher/latest/link/link-library.html),
that works, too:

```dart
Link(
  uri: Uri.parse('/page2'),
  builder: (context, followLink) => TextButton(
    onPressed: followLink,
    child: const Text('Go to page 2'),
  ),
),
```

If the `Link` widget is given a URL with a scheme, e.g. `https://flutter.dev`,
then it will launch the link in a browser. Otherwise, it'll navigate to the link
inside the app using the built-in navigation system.

You can also navigate to a [named route](#named-routes), discussed below.