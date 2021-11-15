---
title: Extra Parameter
date: Last Modified 
permalink: /extra-param/index.html
eleventyNavigation:
  key: Extra Parameter
  order: 23
---
In addition to passing along path and query parameters, you can also pass along
an extra object as part of your navigation, e.g.

```dart
void _tap(BuildContext context, Family family) =>
  context.go('/family', extra: family);
```

This object is available during the `pageBuilder` function as `state.extra`:

```dart
GoRoute(
  path: '/family',
  pageBuilder: (context, state) => MaterialPage<Family>(
    key: state.pageKey,
    child: FamilyPage(family: state.extra! as Family),
  ),
),
```

The `extra` object is useful if you'd like to simply pass along a single object
to the `pageBuilder` function w/o passing an object identifier via a URI and
looking up the object from a store. _However_, this object cannot be used to
create a dynamic link, cannot be used in deep linking and so is _not
recommended_ for those cases.