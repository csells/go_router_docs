---
title: Parameters
date: Last Modified 
permalink: /parameters/index.html
eleventyNavigation:
  key: Parameters
  order: 14
---
The route paths are defined and implemented in [the path_to_regexp
package](https://pub.dev/packages/path_to_regexp), which gives you the ability
to include parameters in your route's `path`: 

```dart
final _router = GoRouter(
  routes: [
    GoRoute(
      path: '/family/:fid',
      pageBuilder: (context, state) {
        // use state.params to get router parameter values
        final family = Families.family(state.params['fid']!);

        return MaterialPage<void>(
          key: state.pageKey,
          child: FamilyPage(family: family),
        );
      },
    ),
  ],
  errorPageBuilder: ...,
]);
```

You can access the matched parameters in the `state` object using the `params`
property.